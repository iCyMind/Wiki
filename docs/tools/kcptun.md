# Kcptun

用 [kcptun](https://github.com/xtaci/kcptun) 加快传输速度，本文配合 shadowsocks 使用。代理链为:  
ss-local(1090)  ->  kcptun-client(1070)  ->  | ...wall... |  ->  kcptun-server(7979)  ->  ss-server(8989)

## 参数
ss 和 kcptun 可以配置在任何可用的端口。为说明方便，假设服务器已经搭设好 kcptun 和 shadowsocks 服务，kcptun-server 监听服务器的 7979 端口，ss-server 配置在 8989 端口。
本文打算将 ss-local 配置在 1090 端口，将 kcptun 配置在 1070 端口。

## 配置 ss-local
- 将 ss-local 配置为监听本地 1090 端口，所有抵达这个端口的数据都转发到本地 kcptun 客户端所在的 1070 端口。即 shadowsocks 的配置文件 shadowsocks.json 内容为：
```json
{
    "server":       "127.0.0.1",
    "server_port":  1070,
    "local_port":   1090,
    "password":     "paaasssword",
    "timeout":      300,
    "method":       "methoooddd"
}
```
其中 password 字段和 method 字段要和 shadowsocks 服务端一致。只有用同样的加密方式，数据经过 kcptun-client -> | wall | -> kcptun-server 最终抵达 ss-server 时，才能被 ss-server 正确解密。

- 运行: `ss-local -c shadowsocks.json -u`

## 配置kcptun-client
- 首先去 [地址](https://github.com/xtaci/kcptun/releases) 下载对应系统的最新版本，解压得到两个可执行文件，分别对应服务器版本和客户端版本。这里只需要客户端版本 client_*
- 新建一个 kcptun 的配置文件 kcptun.json ,  内容为：
```json
{
    "remoteaddr":   "123.123.123.123:7979",
    "localaddr":    ":1070",
    "crypt":        "xor",
    "nocomp":       "true"
}
```
其中123.123.123.123为服务器地址， 7979 为 kcptun 服务端地址，1070 为 kcptun 客户端监听的地址。

- 运行: `client_linux_amd64 -c kcptun.json`

## 使用
将系统代理或者浏览器代理地址设置为 127.0.0.1:1090 ，打开 youtube 看看，速度应该会比之前有所改善。

广州 50M 电信光纤，测试时间工作日 20:30 ，单纯用 shadowsocks 看 youtube，速度在 350 Kbps 以下:

![pure-shadowsocks](http://ww4.sinaimg.cn/large/5d4db8f9gw1f9e1axyc2kj20ht09sdh8.jpg)


配上kcptun , 速度会慢慢爬到最高 **9000 Kbps** 左右:

![shadowsocks + kcptun](http://ww2.sinaimg.cn/large/5d4db8f9gw1f9e1bcpopwj20hs0a0dhe.jpg)

## 配置在路由器上
kcptun 由为 openwrt 路由器编译的版本 [openwrt-kcptun](https://github.com/bettermanbao/openwrt-kcptun), 但是该版本有压缩功能的 [bug](https://github.com/bettermanbao/openwrt-kcptun/issues/8)，因此如果需要在路由器上配置客户端，要求服务器端和客户端都关闭 kcptun 的压缩功能：在 kcptun.json 中添加一行 `"nocomp": true` 。

在我的 wndr4300 上运行，如果加密方式选择默认的 aes , 那么开网页路由器的 cpu 占用达 40% ， 看墙外的视频的话，cpu 使用率分分钟上 95% 。把加密方式改为最简单的 xor 可以把看视频时 cpu 使用率降到 60% 左右。但是性能比起电脑上，速度还是少了至少有 50%，但还是比单纯使用 shadowsocks 快 很多倍。同一时段下进行简单的测试(非常不严谨的测试)，osx(kcptun)/openwrt(kcptun)/openwrt(shadowsocks) 的速度大致为 10000Kbps/3000Kbps/300Kbps

## 禁忌
在用手机流量且流量不多的情况下，千万别在手机上使用 kcptun 模式，1000M 的实际数据，将会耗用你至少 1000 * 1.5 = 1500M 的流量。
