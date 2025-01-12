# ssh 隧道配置

## 端口转发

### 本地端口转发

1. 概念
把本地主机端口通过待登录主机端口转发到远程主机端口

2. 应用场景
    一般来讲，云主机的防火墙默认只打开了22端口，如果需要访问6379端口的话，需要改防火墙。为了保证安全，防火墙需要配置允许访问的IP地址。但是，本地公网IP通是网络提供商动态分配的，是不断变化的。这样的话，防火墙配置需要经常修改，就会麻烦

3. 示例
    假定host1是本地主机，host2是远程主机。由于种种原因，这两台主机之间无法连通。但是，另外还有一台host3，可以同时连通前面两台主机，在host1上执行命令

```bash
ssh -NTLf [host1:]2122:host2:22 host3
# L后面的三个参数分别是 本地端口:目标主机:目标主机端口，意思是指定ssh绑定地端口的2122，指定host3将所有数据转发到host2 的22端口,N表示只进行远程接，不打开远程shell，T表示不为这个连接分配tty,f表示连接成功后，转入后台行，
```
### 远程端口转发

**示例**
```bash
# 假定host1是本地主机，host2是远程主机。由于种种原因，这两台主机之间无法连通。但是，另外还有一台host3，可以同时连通前面两台主机,但是此时的host3是内网机器，host1无法连接到host3，在host3上执行命令
ssh -R 2122:host2:22 host1
# R后面的三个参数分别是 远程主机端口:目标主机:目标主机端口，意思是将host1监听 自己的2122端口，然后将所有数据经过host3转发到host2
```


### 动态端口转发
**示例**
```bash
# 假定host1是本地主机，host2是远程主机。由于种种原因，这两台主机之间无法连通。但是，另外还有一台host3，可以同时连通前面两台主机,此时我们可以使用本地端口转发，但是分别为每个端口创建本地端口会非常麻烦，此时就可以用到动态端口转发，不过，本地需要发起的请求，都需要socket代理转发搭配ssh绑定的端口, 在host1上执行
ssh -D host2:2000 host3
```