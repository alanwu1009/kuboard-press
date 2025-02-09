---
vssueId: 150
description: Kuboard_的日志界面和终端界面都使用了WebSocket与服务器进行通信_在部分情况下_可能出现连通性问题_本文描述了一种解决此问题的办法
meta:
  - name: keywords
    content: Kubernetes 管理界面, Kuboard
---

# 日志终端访问的问题

极少数情况下，您可能能够正常访问 Kuboard 和使用 Kuboard 的各项功能，但是，访问 Kuboard 提供的日志界面和终端界面时，始终会出现弹窗提示，并将您指引到了现在的这个页面上来。本文描述了如何排查 Kuboard 日志/终端界面不能访问的问题

## 第一步

Kuboard 日志界面和终端界面都使用了 websocket 与服务器端通信，正常情况下，会工作得很好，但是当出现如下几种情况时，websocket 的连接就会出现问题：
* 您所访问的容器已经停止
* 您当前使用的浏览器不支持 WebSocket，推荐使用最新版本的 chrome 浏览器，也可以尝试最新版本的 firefox

如果您还有问题，请尝试：
* 清空浏览器缓存，重新登录 Kuboard

## 第二步

当您排除了上述两个问题之后，剩下极有可能的情况就是：
* 您访问服务器时，网络链路上存在代理，比如：
  * 您配置了 Nginx 反向代理，通过 Nginx 将请求转发到 Kuboard 的节点端口 32567
  * 您的浏览器设置了代理程序，并通过代理访问网站内容
  * 您使用了科学上网的工具
  * 您通过 VPN 接入到服务器所在的网络，然后访问 Kuboard 的节点端口 32567
  * 您的网络运营商（如长城宽带、小区宽带、电力猫等）为了节省出口带宽，对所有的 HTTP 服务都做了代理和缓存

此时，您可以尝试使用 kubectl port-forward 的方式来访问 Kuboard。具体步骤如下：

* 请参考 [在客户端电脑安装 kubectl](/install/install-kubectl.html)
* 在客户端电脑上执行端口转发命令，此命令将监听您客户端机器的 8000 端口，并将请求转发到 kuboard 所在 Pod 的 80 端口

  ``` sh
  kubectl port-forward svc/kuboard -n kube-system 8000:80
  ```
* 在 chrome 打开地址 `http://localhost:8000/
  
  登录重试，此时应该能够正常访问 kuboard 的日志界面和终端界面。

::: tip 如果还解决不了
请参考本文末尾的方式联系 Kuboard 团队
:::
