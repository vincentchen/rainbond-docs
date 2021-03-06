---
title: 服务操作
summary: 用户针对服务可以做哪些操作
toc: true
asciicast: true
---

本文讲解服务的行为，也就是说用户可以对服务做哪些控制。

## 服务基本操作

构建、启动、关闭、重启、更新、删除和访问是服务最基本和最常用的操作，根据服务的[不同状态](./app-properties.html)，用户可以执行不同的操作，下面会针对每一个操作介绍功能和操作后所触发的事件。


### 1.1 构建

> 适用场景：服务的任何状态（除应用市场安装的服务且该服务在应用市场无更新）

针对不同类型的服务，触发 `构建` 操作后，有着不同的含义，下表针对不同类型的服务加以说明：

| 服务类型        | 说明                                                                                                                                               |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 各语言源代码    | 拉取最新源代码，通过[rbd-chaos](./app-properties.html)调用[builder](https://github.com/goodrain/builder)进行构建，上线服务 |
| Dockerfile 源码 | 拉取最新源代码，通多 docker build 创建 Docker 镜像，push 镜像到内部镜像仓库，上线服务                                                              |
| Docker 镜像     | 重新拉取指定镜像地址的镜像，推送到本地镜像仓库，上线服务                                                                                           |
| 云市应用        | 重新拉取云市服务镜像/服务包，推送到本地镜像仓库，上线服务                                                                                          |

{{site.data.alerts.callout_info}}

- Dockerfile 源码类服务就是将 Dockerfile 及所需要的文件放到 代码仓库（Git/Svn），通过源代码创建的服务。
- 构建后，如果一切顺利，服务会自动切换为新版本并上线，构建操作默认并更新升级，也可在其他设置中去设置构建后不升级流程。
- 单节点服务构建后服务会有 3~10 秒的中断（根据服务启动时间），多节点服务不受影响。
- 处于关闭状态的服务，触发构建操作后，如果构建正常，平台会将服务运行起来。

{{site.data.alerts.end}}

### 1.2 更新

> 使用场景：运行中服务

触发更新操作后，平台会将现有的服务更新，发送更新请求给数据中心，会升级到最新的服务状态，即改变了某个环境变量或连接信息，都可以触发`更新`操作来处理。

<img src="https://grstatic.oss-cn-shanghai.aliyuncs.com/images/docs/5.0/user-manual/1544171271240.jpg" width="85%" />


### 1.3 启动

> 使用场景：首次构建成功，并处于关闭状态的服务

启动操作会启动上一次成功构建的镜像，如果运行正常会自动上线。

启动后可以在服务概览页面的 `操作日志` 看到平台调度与处理服务的详细操作日志，当调度完成后，服务就进入启动阶段，这时候可以通过 `日志` 页面查看服务的启动日志。

- 服务概览页的操作日志
  <img src="https://static.goodrain.com/images/docs/3.6/user-manual/manage/app-start-log01.png" width="100%" />

* 服务日志页面
  <img src="https://static.goodrain.com/images/docs/3.6/user-manual/manage/app-start-log02.png" width="100%" />

### 1.4 关闭

> 使用场景：运行中或运行异常的服务

触发关闭操作后，平台会将服务的从负载均衡下线并关闭服务容器，多个节点的服务会滚动下线。

### 1.5 重启

> 使用场景：运行中或运行异常的服务

触发重启操作后，平台会将现有的服务重启，对于无状态服务，重启过程滚动进行，有状态服务，将关闭全部实例后再启动。

{{site.data.alerts.callout_danger}}

- 重启服务并不会更新服务代码或镜像，需要和`构建`操作区分。
- 重启操作会中断有状态服务和单节点无状态应用的服务。

{{site.data.alerts.end}}

### 1.6 访问

> 使用场景：运行中的服务 && （打开了对外服务 | 对内服务的端口）

针对不同协议的服务，点击访问按钮后所触发的命令也不一样：

| 服务协议 | 点击访问按钮后的操作                                                           |
| -------- | ------------------------------------------------------------------------------ |
| HTTP     | 浏览器新开窗口，打开服务的默认域名，如果绑定多个域名，会显示域名列表供用户选择 |
| TCP      | 弹出访问信息窗口                                                               |

- HTTP 协议服务

<img src="https://static.goodrain.com/images/docs/3.6/user-manual/manage/app-open-http.gif" width="85%" />

- TCP 协议服务

<img src="https://static.goodrain.com/images/docs/3.6/user-manual/manage/app-open-tcp.gif" width="85%" />

### 1.7 删除

> 使用场景：关闭状态下的服务

`删除`是将平台现有的改服务进行删除，如果您想要废弃改服务，可以将其删除，`删除`服务会将和该服务相关的其他设置一起删除，例如：域名，服务与应用关系，环境变量等等。

<img src="https://grstatic.oss-cn-shanghai.aliyuncs.com/images/docs/5.0/user-manual/1544171708320.jpg" width="85%" />
