---
title: 使用Istio和SpringBoot进行RPC调用
top: false
date: 2022-06-24 11:29:19
tags:
- istio
- springboot
categories:
- 技术资料
---

# TL&DR
Kubernetes相关的云原生技术非常火热，相关的service mesh和istio前沿技术也流行起来。

尝试使用Istio和SpringBoot结合的方式来做内部服务间的rpc调用。

## 术语
- rpc： remote procedure call 