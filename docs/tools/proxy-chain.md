# 翻墙工具链

## 服务端

用 docker 快速在服务器上搭建起 shadowsocks 和 kcptun 服务：

```bash
docker run -d \
    --restart always \
    -p 7979/tcp \
    -p 7979/udp \
    -p 8989/tcp \
    -p 8989/udp \
    -e PASSWD="what-ever" \
    -e METHOD="chacha20" \
    icymind/ss-kt
```

该镜像启动了 shadowsocks 和 kcptun ，各自的参数为, shadowsocks:

`/usr/bin/ss-server -s "0.0.0.0" -p 8989 -k %(ENV_PASSWORD)s -m %(ENV_METHOD)s -t 300 -u`

kcptun：

`/opt/kcptun/server_linux_amd64 -l ":7979" -t "127.0.0.1:8989"`

shadowsocks 运行在 8989 端口，kcptun 运行在 7979 端口。kcptun 的 target 指向 shadowsocks。

记得打开防火墙的相应端口：

```bash
sudo iptables -A INPUT -p tcp --dport 7979 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 7979 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8989 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 8989 -j ACCEPT
```

## 客户端

### 1. Router

#### openwrt-shadowsocks-libev

- ss-tunnel 进行 dns 查询: ` ss-tunnel -s host-ip-address -p 8989 -l 5555 -m chacha20 -k what-ever -L 8.8.8.8:53 -u -v `
- ss-redir 进行透明代理：` ss-redir -s host-ip-address -p 8989 -l 1080 -m chacha20 -k what-ever -t 300 -u -v`
- 可以写个文件方便管理:

    ```bash
    root@OpenWrt:~# cat /etc/init.d/ss

    #!/bin/sh /etc/rc.common
    # Copyright (C) 2006-2011 OpenWrt.org
    START=95

    SERVICE_USE_PID=1
    SERVICE_WRITE_PID=1
    SERVICE_DAEMONIZE=1

    start() {
        # ss-tunnel cannot work fine with kcptun.
        service_start /usr/bin/ss-tunnel -b 0.0.0.0 -l 5353 -L 8.8.8.8:53 -c /etc/ss.json -u
        service_start /usr/bin/ss-redir  -b 0.0.0.0 -c /etc/ss.json -u
    }

    stop() {
        service_stop /usr/bin/ss-tunnel
        service_stop /usr/bin/ss-redir
    }
    ```

#### iptables

利用 iptables ，可以把 ip 集合中的数据导向 shadowsocks 的端口。

```bash
root@OpenWrt:~# cat /etc/firewall.user

# This file is interpreted as shell script.
# Put your custom iptables rules here, they will
# be executed with each firewall (re-)start.

# Internal uci firewall chains are flushed and recreated on reload, so
# put custom rules into the root chains e.g. INPUT or FORWARD or into the
# special user chains, e.g. input_wan_rule or postrouting_lan_rule.

ipset -N SHADOWSOCKS hash:ip

#telegram ips
#ipset add SHADOWSOCKS 149.154.160.0/20
#ipset add SHADOWSOCKS 149.154.164.0/22
#ipset add SHADOWSOCKS 91.108.4.0/22
#ipset add SHADOWSOCKS 91.108.56.0/22

#redir packages from router itself
iptables -t nat -A OUTPUT -p tcp -m set --match-set SHADOWSOCKS dst -j REDIRECT --to-port 1080
iptables -t nat -A OUTPUT -p udp -m set --match-set SHADOWSOCKS dst -j REDIRECT --to-port 1080

# redir packages from clients
iptables -t nat -A PREROUTING -p tcp -m set --match-set SHADOWSOCKS dst -j REDIRECT --to-port 1080
iptables -t nat -A PREROUTING -p udp -m set --match-set SHADOWSOCKS dst -j REDIRECT --to-port 1080

# redir packages to telegram ip adress
#iptables -t nat -A PREROUTING -p tcp -d 149.154.160.0/20 -j REDIRECT --to-ports 1080
#iptables -t nat -A PREROUTING -p tcp -d 149.154.164.0/22 -j REDIRECT --to-ports 1080
#iptables -t nat -A PREROUTING -p tcp -d 91.108.4.0/22 -j REDIRECT --to-ports 1080
#iptables -t nat -A PREROUTING -p tcp -d 91.108.56.0/22 -j REDIRECT --to-ports 1080
```

#### dnsmasq-full

利用 dnsmasq-full ，可以让指定域名通过 ss-tunnel 进行解析，并把解析到的 ip 地址加入某个 ipset。

```bash
root@OpenWrt:/etc/dnsmasq.d# cat over_the_wall.conf

server=/0rz.tw/127.0.0.1#5353
ipset=/0rz.tw/SHADOWSOCKS
server=/0to255.com/127.0.0.1#5353
ipset=/0to255.com/SHADOWSOCKS
server=/1-apple.com.tw/127.0.0.1#5353
ipset=/1-apple.com.tw/SHADOWSOCKS
server=/1000giri.net/127.0.0.1#5353
ipset=/1000giri.net/SHADOWSOCKS
```

#### KCPTUN

在路由器上运行 kcptun 对 shadowsocks 进行加速，受限于路由器的性能，加速效果不是很好，详情见 [kcptun](tools/kcptun) 。可以考虑将路由器上需要翻墙的数据导向 Mac，让 Mac 进行加速。缺点是必须要保持 Mac 开机。

### 2. OSX

- Install shadowsocks with: `brew install shadowsocks-libev`
- ss-local 进行转发：`ss-local -s host-ip-address -p 8989 -l 1080 -k what-ever -m chacha20 -t 300 -u -v`
- 也可以加一层 kcptun 进行加速:

    ```bash
    ss-local -s localhost -p 1070 -l 1090 -k what-ever -m chacha20 -t 300 -u -v
    kcptun -l ":1070" -r "host-ip-address:7979"
    ```

- 为了区分墙内墙外，可以再套一层 [cow](https://github.com/cyfdecyf/cow) 进行智能翻墙
- 有些终端软件不能用 socks 代理，可以用 privoxy 转化为 http 代理

#### 透明代理

OSX 没有 iptables ， 因此无法将被墙域名解析对应的 ip 放入 ipset ，然后对 ipset 进行转发。基于底层防火墙都是基于 ip 进行处理的，因此拿到域名的 ip 是刚需。

问题是，拿到一个域名的 ip 很简单， 但是拿到一个域名都所有子域名的 ip 是比较多余的(工具 [subbrute](https://github.com/TheRook/subbrute) 可以获取一个域名的所有子域名)，很多子域名你根本就用不上。

暂时想到两个思路：

1. 在路由器上长期记录访问的域名，收集这些域名的 ip 地址。然后在 OSX 上用 pf 将这些地址的数据包导向 shadowsocks 端口。
2. 在 Mac 上运行一个虚拟机(docker or virtualbox-headless)，这个虚拟机将担任类似 openwrt 的职责。注意虚拟机有多个 ip ，因此 ss-local 中 local_address 参数要写 0.0.0.0

### 3. Android

Shadowsocks for android.

### 4. iOS
