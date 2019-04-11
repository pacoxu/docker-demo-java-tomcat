# 代码介绍

构建的镜像  daocloud.io/dcelib/nonrootimage

DCS 链接： http://dashboard.daocloud.io/orgs/dcelib/packages/477296e9-802b-48b2-b197-1a09ccee019d

构建命令：

```
docker build . -t daocloud.io/dcelib/nonrootimage:dev
```

## 运行效果

```
$ sudo docker run --rm daocloud.io/dcelib/nonrootimage id
uid=8877(john) gid=8877(john) groups=8877(john)
```

# 介绍

 容器中以指定用户运行应用程序

一般场景下，我们会使用root账户来直接运行Docker容器，这也意味着我们可以在容器中进行任何操作，包括安装软件包，修改配置文件，绑定特权端口，修改权限，创建用户或组等。如果容器中应用或服务本身存在漏洞，一旦被入侵，黑客可以通过容器逃逸机制直接威胁到相应的宿主机安全。为了解决这个问题，我们需要把主进程以非root用户运行

```
RUN groupadd -g 1001 appgroup && \
     useradd -r -u 1001 -g appgroup appuser

USER appuser
```

# Docker 容器的 root 跟主机 root 还是有很大区别的

http://guide.daocloud.io/dcs/docker-root-9153994.html
上文列举了 Docker 容器中的 root 用户的实际权限，Docker 容器的 root 用户不具备 CAP_SYS_ADMIN，磁盘限额操作、mount 操作、创建进程新命名空间等均无法实现；比如由于没有 CAP_NET_ADMIN，网络方面的配置管理也将受到管制。因此，默认情况下，Docker 容器中的 root 用户并没有以往我们想象得那么能力超群，Docker 依然对其存在限制，这样设计的出发点之一自然是安全。
