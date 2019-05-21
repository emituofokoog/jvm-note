# [Redis详解（一）------ redis的简介与安装](https://www.cnblogs.com/ysocean/p/9074353.html)

### bin/ 命令

1. redis-server：Redis服务器
2. redis-cli：Redis命令行客户端
3. redis-benchmark：Redis性能测试工具
4. redis-check-aof：AOF文件修复工具
5. redis-check-rdb：RDB文件检查工具

### 启动redis

```
../bin/redis-server  ../conf/redis.conf
```

### 关闭redis

```
1. redis-cli shutdown 安全关闭，但是只适用于没有配置密码的场景（一般情况下不会给Redis设置密码）
2. kill -9 pid：强制关闭，可能会造成Redis内存数据丢失。
```

