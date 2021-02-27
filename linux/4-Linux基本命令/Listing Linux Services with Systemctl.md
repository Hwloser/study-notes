# Listing Linux Services with Systemctl

在Linux中，服务是在后台运行的程序。服务可以按需启动，也可以在启动时启动。

如果您使用Linux作为主要的操作系统或开发平台，您将处理不同的服务，如webserver、ssh或cron。在调试系统问题时，了解如何列出正在运行的服务或检查服务状态非常重要。

大多数最近的Linux发行版都使用systemd作为默认的init系统和服务管理器。

Systemd是一套用于管理Linux系统的工具。它用于启动计算机、管理服务、自动装载文件系统、记录事件、设置主机名和其他系统任务。

## Listing Linux Services

Systemd uses the concept of units, which can be services, sockets, mount points, devices, etc. Units are defined using text files in `ini` format. These files include information about the unit, its settings, and commands to execute. The filename extensions define the unit file type. For example, system service unit files have a `.service` extension.

`systemctl` is a command-line utility that is used for controlling systemd and managing services. It is part of the systemd ecosystem and is available by default on all systems.

To get a list of all loaded service units, type:

```bash
sudo systemctl list-units --type service
```

每一行的内容如下所示：

- `UNIT` - service unit 的名称。
- `LOAD` - name所示的service unit 是否被加载入内存中。
- `ACTIVE` - 高级unit 文件的激活状态，对应的状态：`active`，`reloading`，`inactive`，`failed`，`activating`，`deactivating`。它是`SUB`列的推广。
- `SUB` - 低级单位文件激活状态。此字段的值取决于单位类型。例如，服务类型的单元可以处于以下状态之一：dead, exited, failed, inactive, or running。
- `DESCRIPTION` - Short description of the unit file。

默认情况下，systemctl将会显示全部被装载的units。

```
systemctl status [<service_name>.service]
```

- `Loaded` - Whether the service unit has been loaded and the full path to the unit file. It also shows whether the unit is enabled to start on boot time.
- `Active` - Whether the service is active and running. If your terminal supports colors and the service is active and running, the dot (`●`) and “active (running)” part will be printed in green. The line also shows how long the service is running.
- `Docs` - The service documentation.
- `Process` - Information about the service processes.
- `Main PID` - The service PID.
- `Tasks` - The number of tasks accounted for the unit and the tasks limit.
- `Memory` - Information about used memory.
- `CGroup` - Information about related Control Groups.