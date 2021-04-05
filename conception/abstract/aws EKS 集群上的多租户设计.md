# aws EKS 集群上的多租户设计

## 明确问题

多租户机制意味着不同类型的工作负载或者团队可以共享同一套集群资源，并且工作负载与团队之间还需要实现资源的逻辑隔离或者物理隔离。资源隔离的原因多种多样，可能是为了保障安全，要求各个团队只能运行与自己相关的工作负载，而不影响其他团队的工作负载；也可以是通过建立网络隔离的方式确保不同应用程序之间互不干扰（在默认情况下，Kubernetes所有命名空间内的所有Pod都能够相互通信）。另一大原因是如果多个租户共享同一基础设施，那么基础设施资源（包括CPU、内存、网络等）需要平均分配给不同工作负载。特别是对提供软件解决方案的SaaS厂商而言，在单一集群上运行多个租户确实能够极大提升基础设施的资源利用率，但同时也给各租户之间的资源隔离和保护机制提出了更高的要求。

## Kubernetes的原生多租户解决方案

Kubernetes提供一系列原生Kubernetes API与资源，用于帮助在单一集群内建立起多租户体系。

### 计算隔离

Kubernetes说明文档中将命名空间定义为“一种在多个用户之间分配集群资源的方法”，这也使其成为多租户机制的基础。大多数Kubernetes资源对象都属于一个命名空间。在下图中，我们定义了两个命名空间，每个命名空间各自运行一组彼此隔离的资源对象。

![img](aws%20EKS%20%E9%9B%86%E7%BE%A4%E4%B8%8A%E7%9A%84%E5%A4%9A%E7%A7%9F%E6%88%B7%E8%AE%BE%E8%AE%A1.assets/multi-tenant-design-considerations-for-amazon-eks-clusters1.png)

虽然K8S中并没有租户这样的概念，命名空间本身也不提供工作负载或用户的隔离，但是K8S除了以命名空间作为基础的资源隔离单位，还提供了基于 RBAC 的权限管理方式：RBAC用于定义谁能够在Kubernetes API上执行的操作。这种授权机制可以通过ClusterRole作用于整个集群，也可以通过Role作用于某个命名空间。例如，我们可以定义一个名为namespace1-admin的角色，该角色负责为管理员提供对namespace1命名空间的访问权限，同时将其与名为admin-ns1的组关联起来，具体操作代码如下：

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: namespace1-admin
  namespace: namespace1
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["batch"]
  resources:
  - jobs
  - cronjobs
  verbs: ["*"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: admins-ns1-rb
  namespace: namespace1
subjects:
- kind: Group
  name: admins-ns1
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: namespace1-admin
  apiGroup: rbac.authorization.k8s.io
```

Amazon EKS通过AWS IAM Authenticator for Kubernetes提供RBAC与IAM的集成功能，可以将IAM用户与角色映射至RBAC组。RBAC是Kubernetes的核心组件，负责控制其他隔离层的权限。我们将在后文中详细讨论。

Kubernetes允许用户对Pod的CPU与内存资源配额做出定义与限制。为了避免节点上发生资源争用，同时改善调度程序的资源分配，开发人员应始终设置资源配额。ResourceQuota资源配额允许用户定义和限制一个命名空间中可以使用的Kubernetes资源或对象的数量。从计算资源的角度来看，用户可以通过资源配额以原生方式定义CPU与内存限制，示例如下：

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: mem-cpu-demo
  namespace: namespace1
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
```

使用资源配额，可以为工作负载分配有限的资源，防止租户之间相互干扰。我们还可以通过[Limit Ranges](https://kubernetes.io/docs/concepts/policy/limit-range)定义每个Pod甚至是容器的默认、最小与最大请求及其限制条件。

租户资源也应该保证与基础设施节点实例之间的隔离。Kubernetes [Pod安全策略](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)（PSP）是集群级的Pod安全策略，自动为集群内的Pod和Volume设置Security Context。Pod Security Policy定义了一系列Pod在系统中运行必须满足的约束条件,以及一些默认的约束值。这要求负责构建多租户集群的管理员通过PSP的Privileged确定Pod中是否可以启用特权模式。通过HostNetwork控制容器是否可以使用所在节点的网络名称空间，防止Pod访问回环设备和潜在的监听网络邻居的活动。自1.13版本起，Amazon EKS正式支持PSP，关于更多详细信息，请参阅[AWS开源博客](https://amazonaws-china.com/blogs/opensource/using-pod-security-policies-amazon-eks-clusters/)。

Kubernetes还提供多种Pod调度方案，在调度的时候考虑Pod与Pod之间的关系，而不只是Pod与Node的关系。例如，[Pod Anti-Affinity](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity)允许用户使用以下设置保证带有特定标签的Pod不会被部署在同一节点之上：

```yaml
apiVersion: v1
kind: Pod
metadata:
  namespace: namespace1
  name: workload-1
  labels:
    team: "team-1"
spec:
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - topologyKey: "kubernetes.io/hostname"
        namespaces:
          - shared-services
        labelSelector:
          matchExpressions:
          - key: "type"
            operator: In
            values: ["monitoring"]
```

在这里，namespace1中带有team: team-1标签的各Pod不会与在shared-services命名空间中拥有monitoring标签类型的Pod被部署在同一节点之上，这是为了防止业务应用与监控应用之间发生干扰。这里还有几点需要注意。首先，为了实现这项功能，我们需要保证在适当的工作负载上应用适当的标签。第二，这类配置可能提高大规模维护工作的难度，因为随着要求的增加与Pod量的提升，过度复杂的调度要求可能导致某些Pod找不到合适的部署节点。最后，Pod亲和性与反亲和性设定会给控制平面带来更高的工作压力，这里不建议大家在包含数百个节点甚至规模更大的集群当中使用。与此类似，我们可以使用nodeSelectors与nodeAffinity将Pod分配给特定节点。

但如果我们希望在调度Pod的时候排除某些节点，让这些节点只在特殊指定时使用，又该怎么办？这种情况就需要使用“[污点（taint）和容忍（toleration）](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)”的功能。在示例中，集群管理员可以使用以下[节点组配置文件](https://eksctl.io/usage/managing-nodegroups/#creating-a-nodegroup-from-a-config-file)在EKS上创建专有的工作节点组，用于监控及管理工作负载：

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata: 
  name: dev-cluster
  region: eu-west-1
nodeGroups:
  - name: ng-monitoring
    instanceType: c5.8xlarge
    desiredCapacity: 2
    taints:
      monitoring: "true:NoSchedule"
```

网络策略还提供更多高级配置，可用于限制多租户集群上的通信。关于更多快速入门以及环境设置指南，请参阅[Amazon EKS说明文档](https://docs.aws.amazon.com/eks/latest/userguide/calico.html)中的相关内容。

我们还可以通过服务网格定义更多的保护机制，甚至可以将保护机制扩展到单一EKS集群之外。Istio是一个非常流行的开源服务网格技术，可提供流量管理、安全保护与可观察性等多种功能。Istio团队在[相关博文](https://istio.io/blog/2018/soft-multitenancy/)中讨论了如何在多租户集群中部署Istio的详细信息。[EKS Workshop](https://eksworkshop.com/servicemesh_with_istio/)还提供关于在EKS上设置Istio的快速入门教程。用户可以使用Istio在不同的网络层内实施对应的身份验证与授权策略。例如，大家可以将用于控制L3/L4网络流量的网络策略与L7层上基于JWT的令牌身份验证结合起来，更好地控制来自不同租户的服务间的通信方式。

### 存储隔离

在使用共享集群时，不同的租户可能需要不同的存储资源类型。Kubernetes提供不同的工具用于管理存储资源。其中最重要的当数[Volume](https://kubernetes.io/docs/concepts/storage/volumes/)，它能够以持久性形式将存储资源接入Pod并管理存储资源的整个生命周期。正如前文“计算”部分所述，这里我们不建议直接挂载由节点提供的存储卷（大家最好在配合PSP的多租户设计中直接禁用掉对本地存储卷的访问）。下面，我们将向大家介绍[PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)(PV)子系统的一些关键功能及其与*PersistentVolumeClaim* (PVC)的关系。

![img](aws%20EKS%20%E9%9B%86%E7%BE%A4%E4%B8%8A%E7%9A%84%E5%A4%9A%E7%A7%9F%E6%88%B7%E8%AE%BE%E8%AE%A1.assets/multi-tenant-design-considerations-for-amazon-eks-clusters2.png)

PersistentVolume（PV）是集群中由管理员配置的网络存储。它是集群中的资源，

，StorageClass也是如此。集群管理员实际上是以集中方式对可用的存储驱动程序、配置以及操作方式做出定义。在Amazon EKS中提供了不同的开箱即用的Storage Classes实现，具体包括Amazon EBS（既作为树内插件，又可作为CSI插件）、Amazon EFS以及FSx for Lustre。而PVC则允许用户根据存储类中的要求为Pod请求存储卷。PVC被定义为一个命名空间资源，并由此对租户的存储访问操作进行控制。管理员可以使用[存储资源配额](https://kubernetes.io/docs/concepts/policy/resource-quotas/#storage-resource-quota)定义属于特定命名空间的存储类。例如，要在命名空间namespace1当中禁用对storagens2存储类的使用，则可按照以下方式使用ResourceQuota：

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: storage-ns1
  namespace: namespace1
spec:
  hard:
    storagens2.storageclass.storage.k8s.io/requests.storage: 0
```

### 多集群间隔离

在实现多租户方面，另外一种可行的选项是使用多个只包含单一租户的Amazon EKS集群。通过这种策略，每个租户都可以在共享AWS账户（或者专供大型企业使用的专有账户）之内拥有只属于自己的Kubernetes集群。

每个集群既可以自行配置，也可以由运维管理团队统一提供，并部署标准化配置。如果选择后一种方法，那么运维管理团队需要高度关注两个问题：集群配置和多集群监控。

## 总结

在本文中，我们从计算、网络与存储的角度介绍了在AWS上实现Amazon EKS多租户设计的几条注意事项。最重要的是，执行本文中提出的上述策略都应综合考虑执行的成本与复杂性。也正因为如此，尽管每个租户使用一个Amazon EKS集群的方法很有吸引力，但是用户必须具备部署和管理多个集群的能力。团队还应该考虑使用Amazon ECS为各个租户集群快速提供服务。最后，AWS还通过[AWS SaaS Factory](https://amazonaws-china.com/partners/saas-factory/tenant-isolation/)为合作伙伴及独立软件供应商建设多租户应用程序提供帮助，需要的朋友请不要错过。