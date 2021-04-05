# NSS 是什么

NSS 是开源软件，和 OpenSSL 一样，是一个底层密码学库，包括 TLS 实现。NSS 并不是完全由 Mozilla 开发出来的，很多公司（包括 Google）和个人都贡献了代码，只是 Mozilla 提供了一些基础设施（比如代码仓库、bug 跟踪系统、邮件组、讨论组）。

NSS 是跨平台的，很多产品都使用了NSS 密码库，包括：

Mozilla 的产品，比如 Firefox、Thunderbird、Firefox OS。

一些开源的软件，比如 Pidgin、Apache OpenOffice、LibreOffice。

Red Hat 的服务器产品，比如 Red Hat Directory Server, Red Hat Certificate System。

其他服务器产品，比如 Apache 的 mod_nss模块，没有发现 Nginx 支持 NSS。

SJES 的 Sun 服务器产品。

NSS 支持的密码学算法标准和应用如下：

SSL&TLS，NSS 计划从 NSS 3.29 版本开始支持 TLS 1.3 协议。

各类 PKCS 公开密码学标准，详细信息可参考 Public Key Cryptography Standards（https://en.wikipedia.org/wiki/PKCS）

Cryptographic Message Syntax，用于 S/MIME（对 MIME 数据进行加密和签名），关于 CMS 标准和 S/MIME 应用了解的不多，所以在《深入迁出HTTPS：从原理到实践》这本书中并没有阐述。

X.509 v3 证书，这是 HTTPS 协议中非常重要的组成部分。

OCSP，是证书非常有效的补充协议，用于在线校验证书的吊销状态（可扩展，还包括其他状态，比如可以包含证书透明度信息）。

各类密码学算法，包括 RSA、DH、ECC、AES、SHA、HMAC 等等。

符合 FIPS 186-2 标准的伪随机生成函数。

NSS 提供了完整的软件开发包，包括密码库、API、命令行工具、文档集（API references、man 帮助、示例代码）。NSS 3.14版本开始，升级到 GPL 兼容的 MPL 2.0 许可证。

NSS 符合 FIPS 140（1&2）标准，FIPS 标准是美国政府定义的一种标准，主要是数据编码的标准。NSS 库也通过了 NISCC TLS/SSL 和 S/MIME 的测试（160万输入数据的测试），NISCC 是英国政府提出的安全标准。

## 关于 NSS API

NSS 代码是分层（Layer）的，分为底层 API 和高层 API，底层 API 完成特定的工作，它不能调用上层的高层 API，而高层 API 主要是包装底层 API，然后给开发者调用。

分层结构如下图：

![img](NSS%20%E6%98%AF%E4%BB%80%E4%B9%88.assets/ljwxml2l0u.jpeg)

NSS API 对外的就是包，包包括一些头文件、包文件等信息，列举几个例子：

NSS 服务提供静态库和动态库，开发者如果想使用静态库，那么只能调用开发者导出的 API，而且这些 API 都能和新版本兼容。

不同的操作系统，NSS 的静态库和动态库有不同的命名约定，见下列表格：

NSS 整个架构图如下：

![](NSS%20%E6%98%AF%E4%BB%80%E4%B9%88.assets/ixiw0f0vp9.jpeg)

NSPR：一个跨平台的底层次函数库，主要作用是为了尽量多的支持各类操作系统，NSS 3.x 版本目前支持 18 个平台，提供 I/O 操作，网络操作函数等基础库。NSPR 是 Mozilla 独立的一个工程。

NSS：主要包含各类密码学库，它包含了一个框架，通过这个框架，开发者和 OEMs 能够提供很多补丁，比如优化密码学操作性能（SSL accelerators、指令集）。

SSL&S/MIME：基于 NSS 实现的应用层协议，最主要的就是 SSL 了。

API 的概要说明见NSS Public Functions，API 详细的调用说明见NSS Reference。

NSS API 都是 C 语言调用的（NSS 本身也是 C 语言开发的），我个人对 Python 比较熟悉，可以使用 python-nss 模块进行开发，后续也会介绍一下。