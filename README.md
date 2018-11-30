docker 常用镜像
=============

一，shadowsocks docker

1，下载镜像需要的文件
```bash
git clone https://github.com/linyy2233/docker.git
```
2，修改shadowsocks server密码和加密方式
```bash
$ cd docker
$ vim vim ss/program/ss/config.json
{
    "server":"0.0.0.0",
    "server_port":53,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"12345671",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

3，修改shadowsocks client连接server密码和上面的密码一致，不需要用到privoxy可以忽略此步骤。privoxy主要是把ss的的代理变成http可以在命令行使用
```bash
$ vim ss/program/ssclient/ssclient.json
{
    "server":"127.0.0.1",
    "server_port":53,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"12345671",
    "timeout":300,
    "method":"aes-256-cfb",
    "workers": 1
}
```

4，生成docker images
```bash
$ cd ss
$ docker build -t ss:v1 ./
```

5，启动容器
```bash
# docker run  -p  3307:3307/udp -p 53:53 -p 18203:18203 ss:v1
```

6，客户端使用
```bash
	6.1 普通客户端直接使用53号端口
	6.2 kcptun使用udp 3307默认配置--key 123456 --crypt aes-128 --mode fast2，修改ss/supervisor.d/kcptun.ini
	6.3 命令行使用 # echo "http_proxy=http://公网IP:18203 https_proxy=http://公网IP:18203 $*" > /usr/local/bin/proxy ;chmod +x /usr/local/bin/proxy
		测试是否正常$ proxy curl linyy.ml/ip 返回上面配置的公网IP为正常
```
