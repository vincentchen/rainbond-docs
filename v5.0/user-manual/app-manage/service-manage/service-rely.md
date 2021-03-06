---
title: 服务操作
summary: 服务依赖
toc: true
asciicast: true
---

## 添加服务依赖服务

### 1.1 服务为什么要依赖其他服务

当一个独立的业务系统不能完成所有功能时，就需要借助其他的服务来实现。如web服务一般都需要数据库存储数据，前端页面展现程序需要调用后端API服务获取数据等等。

建立服务依赖的操作请参考：[添加数据库依赖](./link-db.html)

### 1.2 服务如何连接依赖服务

当服务开启`对内服务`后，其他服务才能通过【添加依赖服务】的方式进行关联，服务与依赖的服务建立起关联后，下一步就是连接依赖服务。


在【依赖】页面中的 【依赖应用信息】可以看到已经依赖的服务：

<img src="https://static.goodrain.com/images/docs/3.6/user-manual/manage/app-link01.png" width="100%" />

#### (1) 获取连接信息

选择其中一个依赖服务，点击【连接信息】会弹出连接信息页面:
<img src="https://static.goodrain.com/images/docs/3.6/user-manual/manage/app-link02.png" width="60%" />

连接信息分为两类:

- 变量名
> 当服务的端口打开<b>对内服务</b>后，会生成一个默认的<b>端口别名</b>这个别名就是该服务的连接信息的前缀。如一个内部的API服务，端口别名是 `USERAPI` 则，其他服务与该API建立关联后，就可以通过 `USERAPI_HOST` 找到API服务的连接IP，通过`USERAPI_PORT` 找到API服务的端口号，如果还需要添加其他的变量名，可以通过 【依赖】--【服务连接信息】添加更多的依赖相关的变量。

- 变量值
> 服务可以通过确定的变量值来连接被依赖（打开对内服务）的服务，我们<b>不推荐</b>使用这种方式连接，这种方式属于硬编码，所有配置都写死到代码中，对于业务安全与程序灵活性都有影响。我们推荐使用<b>环境变量名</b>的方式连接服务。

#### (2) 服务连接依赖服务

当服务添加了依赖，并且查看了连接信息后，下一步就是修改服务的配置，连接依赖的服务，以Springcloud程序为例介绍通过<b>环境变量</b>的形式连接依赖的服务：

`application.yml` 文件

```yml
...
spring:
  data:
    mysql:
      host: ${MYSQL_HOST}
      username: ${MYSQL_USER}
      password: ${MySQL_PASS}
      database: ${MYSQL_DB}
      port: ${MYSQL_PORT}
...
```

其他各类语言都有获取环境变量的方法，如果不想用环境变量，也可以使用直接变量值，但按照<a href="https://12factor.net/zh_cn/config" target="_blank">十二要素</a>原则，我们不推荐使用硬编码的方式连接服务。
