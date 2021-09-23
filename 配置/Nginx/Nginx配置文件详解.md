### 1.配置结构

```
#mian  全局配置

#event配置工作模式以及连接
events { 
    # 默认使用epoll模型
    use epoll;
    # 每个worker允许连接的客户端最大数
    worker_connections  1024;
}

# http模块相关配置{
#   server 虚拟主机配置，可以有多个。{
#        location 路由规则，表达式。
#        updytream 集群，内网服务器
#   }
# }
```



