---
title: 微服务学习
date: 2017-04-24 14:31:11
tags:
- 微服务
categories:
- 技术资料
---
总结微服务相关的一些学习知识，从基本概念到开发、部署涉及到的一些技术和名字。
# 啥是微服务
简单的说，微服务是将一个大型的单块架构拆分为多个细粒度服务的架构。这里我查到了微服务需要的四大要求：
```
- 根据业务模块划分服务种类
- 每个服务可独立部署且相互隔离
- 服务之间通过轻量级API进行通信
- 服务需保证良好的高可用性(24小时)
```
四大特点：
```
- 服务颗粒化
- 责任单一化
- 运行隔离化
- 管理自动化
```
在听qcon时候，有听到过高并发情况下要慎用微服务，有空要仔细搜索下为啥这么说。
# 微服务架构
## 服务颗粒化
究竟多小或者说怎样算微服务，我们主要是根据以下几个方面来考量：
```
根据子系统切分(粗粒度)
根据业务模块切分
根据API切分(细粒度)
```
## 责任单一化
```
一个微服务只做一件事情
一个微服务应该只有一个引起它变化的原因
只要是一件事情，尽管它再复杂，也不要拆分
```
## 运行隔离化
隔离化主要从`部署`和`运行`两个方面来看
**独立部署**
部署A服务的时候，不会影响B服务的运行
**独立运行**
A服务崩溃了，不会影响B服务的运行
## 管理自动化
微服务多了，如何管理它们呢
**自动化部署**
自动地服务升降级，不会影响整个系统的运行状态
**监控与告警**
监控每个微服务的运行状态，及时发出告警
## 什么时候用微服务呢
我们可以考虑几种现实存在的情况
1. 比如传统的单体结构，随着时间推移，项目变的越来越大，很难维护
2. 项目里不止是一种开发语言，考虑到要同时存在java、go、pythen。。。balabala。。。
3. 感觉到经典的一些架构模式太重的时候，比如以前经常说的SOA
4. 修改一个Bug需要平滑升级时候
5. 想对系统进行细粒度监控的时候

## 要设计一个轻量级的微服务架构时考虑啥呢？
引用[InfoQ分享 - 快速搭建轻量级微服务架构](http://www.infoq.com/cn/presentations/building-lightweight-micro-service-architecture )上看到的一个技术分享视频的例子
### 服务拆分
{% asset_img microservice-1.JPG microservice-1 %}
- 根据业务功能拆分服务种类
- 每个服务可独立部署和运行
- 每个服务独享或共享数据库
- 每个服务拥有一组API接口

### 服务注册
{% asset_img microservice-2.jpg microservice-2 %}
当服务启动时，将配置信息注册到服务注册表中，服务注册表需具备高可用性。
### 请求路由
{% asset_img microservice-3.jpg microservice-3 %}
请求需要经过服务网关路由到具体的服务中，服务网关需要能够自动发现可用的服务(服务发现)，服务网关需要具备高可用性且不能成为单点故障。
### 一个轻量级的微服务架构设计图
{% asset_img microservice-4.jpg microservice-4 %}
## 微服务架构的技术选型
微服务开发框架： Spring Boot
微服务容器： Docker
微服务注册表： ZooKeeper
微服务网关： Node.js
微服务自动化部署： Jenkins
具体的一些设计理念我们可以去参考[InfoQ分享 - 快速搭建轻量级微服务架构](http://www.infoq.com/cn/presentations/building-lightweight-micro-service-architecture )，PPT里的解释和具体做法很清楚。
# 一些名词概念
## CI - Continuous Integration 持续集成
{% asset_img bg2015092301.png CI %}

概念：频繁（一天多次）将代码集成到主干。
持续集成的目的，是让产品可以快速迭代，同时还能保持高质量。它的核心措施是，在代码继承到主干之前，必须通过自动化测试，只要有一个测试失败，就不能集成。
- 快速发现错误
- 防止分支大幅偏离主干

## CD - Continuous Delivery 持续交付 / Continuous Deployment 持续部署
{% asset_img bg2015092302.jpg CD %}

持续交付是指，频繁地将软件的新版本，交给质量团队或者用户评审。如果评审通过，代码进入生产阶段。持续交付强调的是，不管怎么更新，软件是随时随地可以交付的。

持续部署是持续交付的下一步，在代码通过评审以后，自动部署到生产环境。持续部署强调的是，代码在任何时刻都是可部署的，可以进入生产阶段。持续部署的前提是能自动化完成测试、构建、部署等步骤。
## SOA - Service Oriented Architecture
面向服务的架构，通过网络与其他组件通信来为其提供服务的应用组件。其目的在于创建具有弹性的分布式应用，有效消除各类复杂的中央组件。
# 微服务要面临的挑战
1. 运维要求较高
系统监控、高可用性、自动化技术
2. 分布式复杂性
网络延迟、系统容错、分布式事务
3. 部署依赖较强
服务依赖、多版本问题
4. 通信成本较高
无状态性、进程间调用

# 参考
- [Matin Fowler的网站](https://martinfowler.com/microservices/)
- [持续集成是什么？](http://www.ruanyifeng.com/blog/2015/09/continuous-integration.html) - 阮一峰
- [redhat白皮书 成功构建微服务架构](https://www.redhat.com/cms/managed-files/mi-microservices-architecture-design-whitepaper-inc0336100lw-201602-a4-zh.pdf)
- [InfoQ分享 - 快速搭建轻量级微服务架构](http://www.infoq.com/cn/presentations/building-lightweight-micro-service-architecture )
