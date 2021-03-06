# wireguard-vpn-scripts-lite

subspace 太难用，部署还麻烦，还不如用 shell 写一套脚本。

这个是简化版，去掉了**从地址池自动分配IP、iptables nat设置、允许访问内网IP段、生成客户端配置的二维码图片、把二维码和clients/xxx.conf给用户发邮件**等功能。

## 1. 生成服务器key

> 需要 root 权限

```
git clone https://github.com/oicu/wireguard-vpn-scripts-lite.git
cp wireguard-vpn-scripts-lite/* /etc/wireguard/
cd /etc/wireguard/
chmod u+x *.sh
./init.sh
```

## 2. 修改服务器IP
```
vi client.tpl
```

修改

Endpoint = 你的服务器公网IP:监听端口

> 端口要和 server.tpl 里的 ListenPort 一致。

## 3. 启动服务
```
./restart.sh
```

## 4. 添加客户端配置

Client1:
```
./addclient.sh myPC 172.16.1.2
```

Client2:
```
./addclient.sh myiPhone 172.16.1.3
```

## 5. 分发客户端配置
```
cat clients/myiPhone.conf
```

如果你安装了`qrencode`，也可以这样生成二维码，然后直接用手机的 WireGuard 客户端扫描：
```
qrencode -t ansiutf8 < clients/myiPhone.conf
```

## 6. 删除客户端配置

> 这里为什么要带 .conf ？因为可以 tab 补全文件名。

```
./delclient.sh myiPhone.conf
```

or

```
./delclient.sh peers/myiPhone.conf
```
