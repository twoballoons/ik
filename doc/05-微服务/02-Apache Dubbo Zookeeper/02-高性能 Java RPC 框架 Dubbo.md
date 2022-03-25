# 01-什么是 Dubbo

# 什么是 Dubbo

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-简介1](https://www.bilibili.com/video/av34187218/)
* [【视频】Dubbo 实现微服务架构-Dubbo-简介2](https://www.bilibili.com/video/av34187257/)

## 概述

Apache Dubbo (incubating) |ˈdʌbəʊ| 是一款高性能、轻量级的开源 Java RPC 分布式服务框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。她最大的特点是按照分层的方式来架构，使用这种方式可以使各个层之间解耦合（或者最大限度地松耦合）。从服务模型的角度来看，Dubbo 采用的是一种非常简单的模型，要么是提供方提供服务，要么是消费方消费服务，所以基于这一点可以抽象出服务提供方（Provider）和服务消费方（Consumer）两个角色。

* 官网：http://dubbo.apache.org/zh-cn
* GitHub：https://github.com/apache/incubator-dubbo

# 02-Dubbo 的服务治理

# Dubbo 的服务治理

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-服务治理](https://www.bilibili.com/video/av34187294/)

## 概述

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typora2062729-5f97eab81f9d55cb.png)

| 特性         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| 透明远程调用 | 就像调用本地方法一样调用远程方法；只需简单配置，没有任何 API 侵入 |
| 负载均衡机制 | Client 端 LB，可在内网替代 F5 等硬件负载均衡器               |
| 容错重试机制 | 服务 Mock 数据，重试次数、超时机制等                         |
| 自动注册发现 | 注册中心基于接口名查询服务提 供者的 IP 地址，并且能够平滑添加或删除服务提供者 |
| 性能日志监控 | Monitor 统计服务的调用次调和调用时间的监控中心               |
| 服务治理中心 | 路由规则，动态配置，服务降级，访问控制，权重调整，负载均衡，等手动配置 |
| 自动治理中心 | 无，比如：熔断限流机制、自动权重调整等                       |

# 03-Dubbo 的核心功能

# Dubbo 的核心功能

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-核心功能与组件角色](https://www.bilibili.com/video/av34187440/)

## 概述

* Remoting：远程通讯，提供对多种 NIO 框架抽象封装，包括“同步转异步”和“请求-响应”模式的信息交换方式。
* Cluster：服务框架，提供基于接口方法的透明远程过程调用，包括多协议支持，以及软负载均衡，失败容错，地址路由，动态配置等集群支持。
* Registry：服务注册中心，服务自动发现: 基于注册中心目录服务，使服务消费方能动态的查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。

# 04-Dubbo 的组件角色

# Dubbo 的组件角色

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-核心功能与组件角色](https://www.bilibili.com/video/av34187440/)

## 概述

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typora0419a8ea9f02f10b2433bbb3b96c9148.png)

| 组件角色  | 说明                                   |
| --------- | -------------------------------------- |
| Provider  | 暴露服务的服务提供方                   |
| Consumer  | 调用远程服务的服务消费方               |
| Registry  | 服务注册与发现的注册中心               |
| Monitor   | 统计服务的调用次调和调用时间的监控中心 |
| Container | 服务运行容器                           |

**调用关系说明：**

* 服务容器 `Container` 负责启动，加载，运行服务提供者。
* 服务提供者 `Provider` 在启动时，向注册中心注册自己提供的服务。
* 服务消费者 `Consumer` 在启动时，向注册中心订阅自己所需的服务。
* 注册中心 `Registry` 返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
* 服务消费者 `Consumer`，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
* 服务消费者 `Consumer` 和提供者 `Provider`，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心 `Monitor`。

# 05-Dubbo Admin 管理控制台

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-管理控制台](https://www.bilibili.com/video/av34187485/)

## 概述

管理控制台为内部裁剪版本，开源部分主要包含：路由规则，动态配置，服务降级，访问控制，权重调整，负载均衡，等管理功能。

GitHub：https://github.com/apache/incubator-dubbo-ops

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181019141753.png)

```Plain Text
# 打包
mvn clean package

# 运行
mvn --projects dubbo-admin-backend spring-boot:run

# 浏览
http://localhost:8080

```

## 遇到的问题处理

### NodeJS

* 现象：使用 `mvn clean package` 构建 DubboAdmin 控制台时会出现 `npm install` 操作
* 解决：新版控制台已改为前后分离模式，前端采用 Vue.js 开发，故需要 NodeJS 支持，请自行安装（运行到此处时会自动下载安装）。官网地址：http://nodejs.cn/
* 其他：配置淘宝镜像加速。官网地址：http://npm.taobao.org/

```Plain Text
# 安装 cnpm 命令行工具
npm install -g cnpm --registry=https://registry.npm.taobao.org

# 安装模块
cnpm install [name]

```

### Will not attempt to authenticate using SASL (unknown error)

* 现象：使用 `mvn --projects dubbo-admin-backend spring-boot:run` 启动 DubboAdmin 控制台时，控制台日志中出现 `Will not attempt to authenticate using SASL (unknown error)` 提示
* 解决：修改 `C:\Windows\System32\drivers\etc\hosts` 文件，增加 `192.168.10.131 ubuntu16` 即可解决

**注意：** 此处的 `192.168.10.131` 为 Zookeeper 地址

### 两处 npm WARN

* 现象：使用 `mvn clean package` 构建 DubboAdmin 控制台时会出现 `npm install` 操作，此时还会出现两处警告，分别为

```Plain Text
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules\fsevents)
```

```Plain Text
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
```

* 解决：从警告说明中可以看出，`fsevents` 模块用于 `{"os":"darwin","arch":"any"}` Mac 系统，当前系统为 `(current: {"os":"win32","arch":"x64"})` Windows 系统，不予理会即可

# 06-第一个 Dubbo 应用程序

# 第一个 Dubbo 应用程序

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-服务提供者](https://www.bilibili.com/video/av34406501/)
* [【视频】Dubbo 实现微服务架构-Dubbo-服务消费者](https://www.bilibili.com/video/av34406576/)

## 概述

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typora0419a8ea9f02f10b2433bbb3b96c9148_2.png)

## 创建服务接口项目

创建一个名为 `hello-dubbo-service-user-api` 的项目，该项目只负责**定义接口**

### POM

```Plain Text
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.funtl</groupId>
    <artifactId>hello-dubbo-service-user-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>
</project>

```

### 定义服务接口

```Plain Text
package com.funtl.hello.dubbo.service.user.api;

public interface UserService {
    String sayHi();
}

```

## 创建服务提供者项目

创建一个名为 `hello-dubbo-service-user-provider` 的项目，该项目主要用于实现接口

### POM

```Plain Text
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.funtl</groupId>
    <artifactId>hello-dubbo-service-user-provider</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>hello-dubbo-service-user-provider</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>0.2.0</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-actuator</artifactId>
            <version>0.2.0</version>
        </dependency>

        <dependency>
            <groupId>com.funtl</groupId>
            <artifactId>hello-dubbo-service-user-api</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.funtl.hello.dubbo.service.user.provider.HelloDubboServiceUserProviderApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```

主要增加了以下依赖：

* `com.alibaba.boot:dubbo-spring-boot-starter:0.2.0`：Dubbo Starter，0.2.0 版本支持 Spring Boot 2.x，是一个长期维护的版本。注：0.1.0 版本已经不推荐使用了，是个短期维护的版本，如果你还在用旧版，请大家尽快升级。
* `com.alibaba.boot:dubbo-spring-boot-actuator:0.2.0`：Dubbo 的服务状态检查
* `com.funtl:hello-dubbo-service-user-api:1.0.0-SNAPSHOT`：刚才创建的接口项目，如果无法依赖别忘记先 `mvn clean install` 到本地仓库。

### 通过 `@Service` 注解实现服务提供方

```Plain Text
package com.funtl.hello.dubbo.service.user.provider.api.impl;

import com.alibaba.dubbo.config.annotation.Service;
import com.funtl.hello.dubbo.service.user.api.UserService;

@Service(version = "${user.service.version}")
public class UserServiceImpl implements UserService {
    @Override
    public String sayHi() {
        return "Hello Dubbo";
    }
}

```

### Application

```Plain Text
package com.funtl.hello.dubbo.service.user.provider;

import com.alibaba.dubbo.container.Main;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HelloDubboServiceUserProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloDubboServiceUserProviderApplication.class, args);
        // 启动 Provider 容器，注意这里的 Main 是 com.alibaba.dubbo.container 包下的
        Main.main(args);
    }
}

```

### application.yml

```Plain Text
# Spring boot application
spring:
  application:
    name: hello-dubbo-service-user-provider

# UserService service version
user:
  service:
    version: 1.0.0

# Dubbo Config properties
dubbo:
  ## Base packages to scan Dubbo Component：@com.alibaba.dubbo.config.annotation.Service
  scan:
    basePackages: com.funtl.hello.dubbo.service.user.provider.api
  ## ApplicationConfig Bean
  application:
    id: hello-dubbo-service-user-provider
    name: hello-dubbo-service-user-provider
    qos-port: 22222
    qos-enable: true
  ## ProtocolConfig Bean
  protocol:
    id: dubbo
    name: dubbo
    port: 12345
    status: server
  ## RegistryConfig Bean
  registry:
    id: zookeeper
    address: zookeeper://192.168.10.131:2181?backup=192.168.10.131:2182,192.168.10.131:2183

# Enables Dubbo All Endpoints
management:
  endpoint:
    dubbo:
      enabled: true
    dubbo-shutdown:
      enabled: true
    dubbo-configs:
      enabled: true
    dubbo-services:
      enabled: true
    dubbo-references:
      enabled: true
    dubbo-properties:
      enabled: true
  # Dubbo Health
  health:
    dubbo:
      status:
        ## StatusChecker Name defaults (default : "memory", "load" )
        defaults: memory
        ## StatusChecker Name extras (default : empty )
        extras: load,threadpool

```

## 创建服务消费者项目

创建一个名为 `hello-dubbo-service-user-consumer` 的项目，该项目用于消费接口（调用接口）

### POM

```Plain Text
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.funtl</groupId>
    <artifactId>hello-dubbo-service-user-consumer</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>hello-dubbo-service-user-consumer</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>0.2.0</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-actuator</artifactId>
            <version>0.2.0</version>
        </dependency>

        <dependency>
            <groupId>com.funtl</groupId>
            <artifactId>hello-dubbo-service-user-api</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.funtl.hello.dubbo.service.user.consumer.HelloDubboServiceUserConsumerApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```

### 通过 `@Reference` 注入 `UserService`

```Plain Text
package com.funtl.hello.dubbo.service.user.consumer.controller;

import com.alibaba.dubbo.config.annotation.Reference;
import com.funtl.hello.dubbo.service.user.api.UserService;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @Reference(version = "${user.service.version}")
    private UserService userService;

    @RequestMapping(value = "hi")
    public String sayHi() {
        return userService.sayHi();
    }
}

```

### Application

```Plain Text
package com.funtl.hello.dubbo.service.user.consumer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HelloDubboServiceUserConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloDubboServiceUserConsumerApplication.class, args);
    }
}

```

### application.yml

```Plain Text
# Spring boot application
spring:
  application:
    name: hello-dubbo-service-user-consumer
server:
  port: 9090

# UserService service version
user:
  service:
    version: 1.0.0

# Dubbo Config properties
dubbo:
  scan:
    basePackages: com.funtl.hello.dubbo.service.user.consumer.controller
  ## ApplicationConfig Bean
  application:
    id: hello-dubbo-service-user-consumer
    name: hello-dubbo-service-user-consumer
  ## RegistryConfig Bean
  registry:
    id: zookeeper
    address: zookeeper://192.168.10.131:2181?backup=192.168.10.131:2182,192.168.10.131:2183

# Dubbo Endpoint (default status is disable)
endpoints:
  dubbo:
    enabled: true

management:
  server:
    port: 9091
  # Dubbo Health
  health:
    dubbo:
      status:
        ## StatusChecker Name defaults (default : "memory", "load" )
        defaults: memory
  # Enables Dubbo All Endpoints
  endpoint:
    dubbo:
      enabled: true
    dubbo-shutdown:
      enabled: true
    dubbo-configs:
      enabled: true
    dubbo-services:
      enabled: true
    dubbo-references:
      enabled: true
    dubbo-properties:
      enabled: true
  endpoints:
    web:
      exposure:
        include: "*"

```

## 启动 Dubbo Admin 控制台

查看是否成功注册服务，效果图如下：

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181022010246.png)

# 07-Dubbo 的负载均衡

# Dubbo 的负载均衡

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-负载均衡](https://www.bilibili.com/video/av34406643/)

## 概述

在集群负载均衡时，Dubbo 提供了多种均衡策略，缺省为 `random` 随机调用。

## 负载均衡策略

### Random LoadBalance

* **随机**，按权重设置随机概率。
* 在一个截面上碰撞的概率高，但调用量越大分布越均匀，而且按概率使用权重后也比较均匀，有利于动态调整提供者权重。

### RoundRobin LoadBalance

* **轮询**，按公约后的权重设置轮询比率。
* 存在慢的提供者累积请求的问题，比如：第二台机器很慢，但没挂，当请求调到第二台时就卡在那，久而久之，所有请求都卡在调到第二台上。

### LeastActive LoadBalance

* **最少活跃调用数**，相同活跃数的随机，活跃数指调用前后计数差。
* 使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大。

### ConsistentHash LoadBalance

* **一致性 Hash**，相同参数的请求总是发到同一提供者。
* 当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，平摊到其它提供者，不会引起剧烈变动。
* 算法参见：http://en.wikipedia.org/wiki/Consistent\_hashing
* 缺省只对第一个参数 Hash，如果要修改，请配置 `<dubbo:parameter key="hash.arguments" value="0,1" />`
* 缺省用 160 份虚拟节点，如果要修改，请配置 `<dubbo:parameter key="hash.nodes" value="320" />`

## 配置

### 服务端服务级别

```Plain Text
dubbo:
  provider:
    loadbalance: leastactive

```

### 客户端服务级别

```Plain Text
dubbo:
  consumer:
    loadbalance: leastactive

```

## 测试负载均衡

### 修改 `UserServiceImpl` 代码为

```Plain Text
package com.funtl.hello.dubbo.service.user.provider.api.impl;

import com.alibaba.dubbo.config.annotation.Service;
import com.funtl.hello.dubbo.service.user.api.UserService;
import org.springframework.beans.factory.annotation.Value;

@Service(version = "${user.service.version}")
public class UserServiceImpl implements UserService {

    @Value("${dubbo.protocol.port}")
    private String port;

    @Override
    public String sayHi() {
        return "Hello Dubbo , i am from port:" + port;
    }
}

```

注入了端口属性，当消费者访问时可以看出是从哪个端口请求的数据。

### 修改负载均衡策略为轮询

```Plain Text
dubbo:
  provider:
    loadbalance: roundrobin

```

### 测试访问

修改端口号并分别启动服务提供者，此时访问服务消费者：http://localhost:9090/hi

浏览器会交替显示：

```Plain Text
Hello Dubbo , i am from port:12345
Hello Dubbo , i am from port:12346

```

## 附：在 IDEA 中配置一个工程启动多个实例

### 步骤一

点击 `Run -> Edit Configurations...`

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181022015716.png)

### 步骤二

选择需要启动多实例的项目并去掉 `Single instance only` 前面的勾

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181022015801.png)

### 步骤三

通过修改 `application.yml` 配置文件的 `dubbo.protocol.port` 的端口，启动多个实例，需要多个端口，分别进行启动即可。

# 08-Dubbo + Kryo 实现高速序列化

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-使用 Kryo 实现高速序列化](https://www.bilibili.com/video/av34446331/)

## Dubbo 中的序列化

Dubbo RPC 是 Dubbo 体系中最核心的一种高性能、高吞吐量的远程调用方式，可以称之为多路复用的 TCP 长连接调用：

* 长连接：避免了每次调用新建 TCP 连接，提高了调用的响应速度
* 多路复用：单个 TCP 连接可交替传输多个请求和响应的消息，降低了连接的等待闲置时间，从而减少了同样并发数下的网络连接数，提高了系统吞吐量

Dubbo RPC 主要用于两个 Dubbo 系统之间的远程调用，特别适合高并发、小数据的互联网场景。而序列化对于远程调用的响应速度、吞吐量、网络带宽消耗等同样也起着至关重要的作用，是我们提升分布式系统性能的最关键因素之一。

Dubbo 中支持的序列化方式：

* dubbo 序列化：阿里尚未开发成熟的高效 java 序列化实现，阿里不建议在生产环境使用它
* hessian2 序列化：hessian 是一种跨语言的高效二进制序列化方式。但这里实际不是原生的 hessian2 序列化，而是阿里修改过的 hessian lite，它是 dubbo RPC 默认启用的序列化方式
* json 序列化：目前有两种实现，一种是采用的阿里的 fastjson 库，另一种是采用 dubbo 中自己实现的简单 json 库，但其实现都不是特别成熟，而且 json 这种文本序列化性能一般不如上面两种二进制序列化。
* java 序列化：主要是采用 JDK 自带的 Java 序列化实现，性能很不理想。

在通常情况下，这四种主要序列化方式的性能从上到下依次递减。对于 dubbo RPC 这种追求高性能的远程调用方式来说，实际上只有 1、2 两种高效序列化方式比较般配，而第 1 个 dubbo 序列化由于还不成熟，所以实际只剩下 2 可用，所以 dubbo RPC 默认采用 hessian2 序列化。

但 hessian 是一个比较老的序列化实现了，而且它是跨语言的，所以不是单独针对 Java 进行优化的。而 dubbo RPC 实际上完全是一种 Java to Java 的远程调用，其实没有必要采用跨语言的序列化方式（当然肯定也不排斥跨语言的序列化）。

最近几年，各种新的高效序列化方式层出不穷，不断刷新序列化性能的上限，最典型的包括：

* 专门针对 Java 语言的：Kryo，FST 等等
* 跨语言的：Protostuff，ProtoBuf，Thrift，Avro，MsgPack 等等

这些序列化方式的性能多数都显著优于 hessian2（甚至包括尚未成熟的 dubbo 序列化）

有鉴于此，我们为 dubbo 引入 Kryo 和 FST 这两种高效 Java 序列化实现，来逐步取代 hessian2。

其中，Kryo 是一种非常成熟的序列化实现，已经在 Twitter、Groupon、Yahoo 以及多个著名开源项目（如 Hive、Storm）中广泛的使用。而 FST 是一种较新的序列化实现，目前还缺乏足够多的成熟使用案例。

**在面向生产环境的应用中，目前更优先选择 Kryo。**

## 启用 Kryo

在 Provider 和 Consumer 项目启用 Kryo 高速序列化功能，两个项目的配置方式相同

### 增加 Kryo 依赖

```Plain Text
<dependency>
    <groupId>de.javakaffee</groupId>
    <artifactId>kryo-serializers</artifactId>
    <version>0.42</version>
</dependency>

```

### 增加配置

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181023080758.png)

### 注册被序列化类

要让 Kryo 和 FST 完全发挥出高性能，最好将那些需要被序列化的类注册到 dubbo 系统中，例如，我们可以实现如下回调接口：

```Plain Text
public class SerializationOptimizerImpl implements SerializationOptimizer {
    public Collection<Class> getSerializableClasses() {
        List<Class> classes = new LinkedList<Class>();
        classes.add(BidRequest.class);
        classes.add(BidResponse.class);
        classes.add(Device.class);
        classes.add(Geo.class);
        classes.add(Impression.class);
        classes.add(SeatBid.class);
        return classes;
    }
}

```

在注册这些类后，序列化的性能可能被大大提升，特别针对小数量的嵌套对象的时候。

当然，在对一个类做序列化的时候，可能还级联引用到很多类，比如 Java 集合类。针对这种情况，我们已经自动将 JDK 中的常用类进行了注册，所以你不需要重复注册它们（当然你重复注册了也没有任何影响），包括：

```Plain Text
GregorianCalendar
InvocationHandler
BigDecimal
BigInteger
Pattern
BitSet
URI
UUID
HashMap
ArrayList
LinkedList
HashSet
TreeSet
Hashtable
Date
Calendar
ConcurrentHashMap
SimpleDateFormat
Vector
BitSet
StringBuffer
StringBuilder
Object
Object[]
String[]
byte[]
char[]
int[]
float[]
double[]

```

由于注册被序列化的类仅仅是出于性能优化的目的，所以即使你忘记注册某些类也没有关系。事实上，即使不注册任何类，Kryo 和 FST 的性能依然普遍优于 hessian 和 dubbo 序列化。

## 为什么需要手动注册

当然，有人可能会问为什么不用配置文件来注册这些类？这是因为要注册的类往往数量较多，导致配置文件冗长；而且在没有好的 IDE 支持的情况下，配置文件的编写和重构都比 Java 类麻烦得多；最后，这些注册的类一般是不需要在项目编译打包后还需要做动态修改的。

另外，有人也会觉得手工注册被序列化的类是一种相对繁琐的工作，是不是可以用 annotation 来标注，然后系统来自动发现并注册。但这里 annotation 的局限是，它只能用来标注你可以修改的类，而很多序列化中引用的类很可能是你没法做修改的（比如第三方库或者 JDK 系统类或者其他项目的类）。另外，添加 annotation 毕竟稍微的“污染”了一下代码，使应用代码对框架增加了一点点的依赖性。

除了 annotation，我们还可以考虑用其它方式来自动注册被序列化的类，例如扫描类路径，自动发现实现 Serializable 接口（甚至包括 Externalizable）的类并将它们注册。当然，我们知道类路径上能找到 Serializable 类可能是非常多的，所以也可以考虑用 package 前缀之类来一定程度限定扫描范围。

当然，在自动注册机制中，特别需要考虑如何保证服务提供端和消费端都以同样的顺序（或者 ID）来注册类，避免错位，毕竟两端可被发现然后注册的类的数量可能都是不一样的。

## 无参构造函数和 Serializable 接口

如果被序列化的类中 `不包含无参的构造函数，则在 Kryo 的序列化中，性能将会大打折扣`，因为此时我们在底层将用 Java 的序列化来透明的取代 Kryo 序列化。所以，`尽可能为每一个被序列化的类添加无参构造函数是一种最佳实践`（当然一个 Java 类如果不自定义构造函数，默认就有无参构造函数）。

另外，Kryo 和 FST 都不需要被序列化类实现 Serializable 接口，但我们还是`建议每个被序列化类都去实现 Serializable 接口，因为这样可以保持和 Java 序列化以及 dubbo 序列化的兼容性`，另外也使我们未来采用上述某些自动注册机制带来可能。

## 附：序列化性能分析与测试

### 测试环境

* 两台独立服务器
* 4 核 Intel(R) Xeon(R) CPU E5-2603 0 @ 1.80GHz
* 8G 内存
* 虚拟机之间网络通过百兆交换机
* CentOS 5
* JDK 7
* Tomcat 7
* JVM 参数 `-server -Xms1g -Xmx1g -XX:PermSize=64M -XX:+UseConcMarkSweepGC`

**注意：** 当然这个测试环境较有局限，故当前测试结果未必有非常权威的代表性

### 测试脚本

和 dubbo 自身的基准测试保持接近，10 个并发客户端持续不断发出请求：

* 传入嵌套复杂对象（但单个数据量很小），不做任何处理，原样返回
* 传入 50K 字符串，不做任何处理，原样返回（TODO：结果尚未列出）

进行 5 分钟性能测试。（引用 dubbo 自身测试的考虑：“主要考察序列化和网络 IO 的性能，因此服务端无任何业务逻辑。**取 10 并发是考虑到 http 协议在高并发下对 CPU 的使用率较高可能会先达到瓶颈。**”）

### Dubbo RPC 中不同序列化生成字节大小比较

序列化生成字节码的大小是一个比较有确定性的指标，它决定了远程调用的网络传输时间和带宽占用。

**针对复杂对象的结果如下（数值越小越好）：**

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181023082512.png)

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typorabytes.png)

### Dubbo RPC 中不同序列化响应时间和吞吐量对比

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181023082647.png)

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typorart.png)

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoratps.png)

### 结论

就目前结果而言，我们可以看到不管从生成字节的大小，还是平均响应时间和平均 TPS，Kryo 和 FST 相比 Dubbo RPC 中原有的序列化方式都有非常显著的改进。

# 09-Dubbo + Hystrix 实现服务熔断

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-使用 Hystrix 实现服务熔断](https://www.bilibili.com/video/av34446940/)

## 熔断器简介

在微服务架构中，根据业务来拆分成一个个的服务，服务与服务之间可以通过 `RPC` 相互调用。为了保证其高可用，单个服务通常会集群部署。由于网络原因或者自身的原因，服务并不能保证 100% 可用，如果单个服务出现问题，调用这个服务就会出现线程阻塞，此时若有大量的请求涌入，`Servlet` 容器的线程资源会被消耗完毕，导致服务瘫痪。服务与服务之间的依赖性，故障会传播，会对整个微服务系统造成灾难性的严重后果，这就是服务故障的 **“雪崩”** 效应。

为了解决这个问题，业界提出了熔断器模型。

Netflix 开源了 Hystrix 组件，实现了熔断器模式，Spring Cloud 对这一组件进行了整合。在微服务架构中，一个请求需要调用多个服务是非常常见的，如下图：

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer201805292246007_2.png)

较底层的服务如果出现故障，会导致连锁故障。当对特定的服务的调用的不可用达到一个阀值（Hystrix 是 **5 秒 20 次**） 熔断器将会被打开。

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer201805292246008.png)

熔断器打开后，为了避免连锁故障，通过 `fallback` 方法可以直接返回一个固定值。

## Dubbo Provider 中使用熔断器

### 在 `pom.xml` 中增加依赖

```Plain Text
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
    <version>2.0.1.RELEASE</version>
</dependency>

```

### 在 Application 中增加 `@EnableHystrix` 注解

```Plain Text
package com.funtl.hello.dubbo.service.user.provider;

import com.alibaba.dubbo.container.Main;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.EnableHystrix;

@EnableHystrix
@SpringBootApplication
public class HelloDubboServiceUserProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloDubboServiceUserProviderApplication.class, args);
        Main.main(args);
    }
}

```

### 在 Service 中增加 `@HystrixCommand` 注解

在调用方法上增加 `@HystrixCommand` 配置，此时调用会经过 Hystrix 代理

```Plain Text
package com.funtl.hello.dubbo.service.user.provider.api.impl;

import com.alibaba.dubbo.config.annotation.Service;
import com.funtl.hello.dubbo.service.user.api.UserService;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import org.springframework.beans.factory.annotation.Value;

@Service(version = "${user.service.version}")
public class UserServiceImpl implements UserService {

    @Value("${dubbo.protocol.port}")
    private String port;

    @HystrixCommand(commandProperties = {
            @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold", value = "10"),
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "2000")
    })
    @Override
    public String sayHi() {
//        return "Hello Dubbo, i am from port:" + port;
        throw new RuntimeException("Exception to show hystrix enabled.");
    }
}

```

### 测试熔断器

此时我们再次请求服务提供者，浏览器会报 500 异常

```Plain Text
Exception to show hystrix enabled.

```

## Dubbo Consumer 中使用熔断器

### 在 `pom.xml` 中增加依赖

```Plain Text
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
    <version>2.0.1.RELEASE</version>
</dependency>

```

### 在 Application 中增加 `@EnableHystrix` 注解

```Plain Text
package com.funtl.hello.dubbo.service.user.consumer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.EnableHystrix;

@EnableHystrix
@SpringBootApplication
public class HelloDubboServiceUserConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloDubboServiceUserConsumerApplication.class, args);
    }
}

```

### 在调用方法上增加 `@HystrixCommand` 注解，并指定 `fallbackMethod` 方法

```Plain Text
package com.funtl.hello.dubbo.service.user.consumer.controller;

import com.alibaba.dubbo.config.annotation.Reference;
import com.funtl.hello.dubbo.service.user.api.UserService;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @Reference(version = "${user.service.version}")
    private UserService userService;

    @HystrixCommand(fallbackMethod = "hiError")
    @RequestMapping(value = "hi")
    public String sayHi() {
        return userService.sayHi();
    }

    public String hiError() {
        return "Hystrix fallback";
    }
}

```

### 测试熔断器

此时我们再次请求服务提供者，浏览器会显示：

```Plain Text
Hystrix fallback

```

至此，Dubbo + Hystrix 配置完成

# 10-Dubbo + Hystrix 熔断器仪表盘

# Dubbo + Hystrix 熔断器仪表盘

## 本节视频

* [【视频】Dubbo 实现微服务架构-Dubbo-使用 Hystrix 熔断器仪表盘](https://www.bilibili.com/video/av34453138/)

## 使用熔断器仪表盘监控

在 Provider 和 Consumer 项目增加 Hystrix 仪表盘功能，两个项目的改造方式相同

## 在 `pom.xml` 中增加依赖

```Plain Text
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
    <version>2.0.1.RELEASE</version>
</dependency>

```

## 在 Application 中增加 `@EnableHystrixDashboard` 注解

```Plain Text
package com.funtl.hello.dubbo.service.user.consumer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.EnableHystrix;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;

@EnableHystrix
@EnableHystrixDashboard
@SpringBootApplication
public class HelloDubboServiceUserConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloDubboServiceUserConsumerApplication.class, args);
    }
}

```

## 创建 `hystrix.stream` 的 Servlet 配置

Spring Boot 2.x 版本开启 Hystrix Dashboard 与 Spring Boot 1.x 的方式略有不同，需要增加一个 `HystrixMetricsStreamServlet` 的配置，代码如下：

```Plain Text
package com.funtl.hello.dubbo.service.user.consumer.config;

import com.netflix.hystrix.contrib.metrics.eventstream.HystrixMetricsStreamServlet;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class HystrixDashboardConfiguration {
    @Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}

```

## 测试 Hystrix Dashboard

浏览器端访问 http://localhost:9090/hystrix 界面如下：

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181023054017.png)

点击 Monitor Stream，进入下一个界面，访问 http://localhost:9090/hi 触发熔断后，监控界面显示效果如下：

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typoraLusifer_20181023054151.png)

## 附：Hystrix 说明

### 什么情况下会触发 `fallback` 方法

| 名字                   | 描述                               | 触发fallback |
| ---------------------- | ---------------------------------- | ------------ |
| EMIT                   | 值传递                             | NO           |
| SUCCESS                | 执行完成，没有错误                 | NO           |
| FAILURE                | 执行抛出异常                       | YES          |
| TIMEOUT                | 执行开始，但没有在允许的时间内完成 | YES          |
| BAD\_REQUEST           | 执行抛出HystrixBadRequestException | NO           |
| SHORT\_CIRCUITED       | 断路器打开，不尝试执行             | YES          |
| THREAD\_POOL\_REJECTED | 线程池拒绝，不尝试执行             | YES          |
| SEMAPHORE\_REJECTED    | 信号量拒绝，不尝试执行             | YES          |

### `fallback` 方法在什么情况下会抛出异常

| 名字               | 描述                           | 抛异常 |
| ------------------ | ------------------------------ | ------ |
| FALLBACK\_EMIT     | Fallback值传递                 | NO     |
| FALLBACK\_SUCCESS  | Fallback执行完成，没有错误     | NO     |
| FALLBACK\_FAILURE  | Fallback执行抛出出错           | YES    |
| FALLBACK\_REJECTED | Fallback信号量拒绝，不尝试执行 | YES    |
| FALLBACK\_MISSING  | 没有Fallback实例               | YES    |

### Hystrix Dashboard 界面监控参数

![image](https://picgo-1301208976.cos.ap-beijing.myqcloud.com//typora20171123110838020.png)

### Hystrix 常用配置信息

#### 超时时间（默认1000ms，单位：ms）

* `hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds`：在调用方配置，被该调用方的所有方法的超时时间都是该值，优先级低于下边的指定配置
* `hystrix.command.HystrixCommandKey.execution.isolation.thread.timeoutInMilliseconds`：在调用方配置，被该调用方的指定方法（HystrixCommandKey 方法名）的超时时间是该值

#### 线程池核心线程数

* `hystrix.threadpool.default.coreSize`：默认为 10

#### Queue

* `hystrix.threadpool.default.maxQueueSize`：最大排队长度。默认 -1，使用 `SynchronousQueue`。其他值则使用 `LinkedBlockingQueue`。如果要从 -1 换成其他值则需重启，即该值不能动态调整，若要动态调整，需要使用到下边这个配置
* `hystrix.threadpool.default.queueSizeRejectionThreshold`：排队线程数量阈值，默认为 5，达到时拒绝，如果配置了该选项，队列的大小是该队列

**注意：** 如果 `maxQueueSize=-1` 的话，则该选项不起作用

#### 断路器

* `hystrix.command.default.circuitBreaker.requestVolumeThreshold`：当在配置时间窗口内达到此数量的失败后，进行短路。默认 20 个（10s 内请求失败数量达到 20 个，断路器开）
* `hystrix.command.default.circuitBreaker.sleepWindowInMilliseconds`：短路多久以后开始尝试是否恢复，默认 5s
* `hystrix.command.default.circuitBreaker.errorThresholdPercentage`：出错百分比阈值，当达到此阈值后，开始短路。默认 50%

#### fallback

* `hystrix.command.default.fallback.isolation.semaphore.maxConcurrentRequests`：调用线程允许请求 `HystrixCommand.GetFallback()` 的最大数量，默认 10。超出时将会有异常抛出，注意：该项配置对于 THREAD 隔离模式也起作用

#### 属性配置参数

* 参数说明：https://github.com/Netflix/Hystrix/wiki/Configuration
* HystrixProperty 参考代码：http://www.programcreek.com/java-api-examples/index.php?source\_dir=Hystrix-master/hystrix-contrib/hystrix-javanica/src/test/java/com/netflix/hystrix/contrib/javanica/test/common/configuration/command/BasicCommandPropertiesTest.java