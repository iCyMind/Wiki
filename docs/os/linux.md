# linux

## 权限管理
对可执行的二进制文件设置 suid bit 可以让普通用户以文件所有者的权限执行. 该方法对脚本文件不管用([原因](https://security.stackexchange.com/questions/194166/why-is-suid-disabled-for-shell-scripts-but-not-for-binaries)), 可以用静态语言将相应功能编译成二进制文件. 例如可以让用户以 root 的权限执行文件 demo: 
```bash
sudo chown root demo
sudo chmod u+s demo
```

[WHAT IS SUID AND HOW TO SET SUID IN LINUX/UNIX?](https://www.linuxnix.com/suid-set-suid-linuxunix/)

## 用户管理
- 添加用户: `useradd -m simon`, -m 参数同时创建用户文件夹
- 设置密码: `passwd simon`
- 将用户添加到 sudo: `adduser simon sudo`
- 更改用户的默认 shell: `chsh -s $(which zsh)`, 如果没有权限, 执行: `sudo chsh -s $(which zsh) simon`

## save iptables rules
- ubuntu > 16.04: `netfilter-persistent save`

## manage service
- ubuntu/debian: `service docker restart`

## install/remove package
- ubuntu
    - apt-get install -y --no-install-recommends sth
    - apt-get autoremove --purge -y sth
    - rm -rf /var/lib/apt/lists/*
- alpine
    - apk --update add git
    - apk --purge del git
    - rm -rf /var/cache/apk/*

## firewall

- [ubuntu-ufw](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04)

## sudoers

It's possible that the only sudo explanation you will ever need is:

%adm ALL=(ALL) NOPASSWD: ALL
This means “any user in the adm group on any host may run any command as any user without a password”. The first ALL refers to hosts, the second to target users, and the last to allowed commands.

## check groups
`groups`

## Network

### list all interface
- ubuntu: `ip link show`

### config network
```bash
cat /etc/network/interfaces

auto lo
iface lo inet loopback

auto ens03
iface ens03 inet dhcp

auto ens08
iface ens08 inet static
    adress 192.168.56.2
    netmask 255.255.255.0
    network 192.168.56.0
    broadcast 192.168.56.255
```

## 启用内核 tcp bbr 拥塞算法

```bash
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9-rc8/linux-image-4.9.0-040900rc8-generic_4.9.0-040900rc8.201612051443_amd64.deb #下载内核
dpkg -i linux-image-4.9.0*.deb  #安装内核
dpkg -l|grep linux-image  #查看内核
apt-get purge (旧的内核名称)  #删除老的内核

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl –p  #保存生效
sysctl net.ipv4.tcp_available_congestion_control  #查看内核是否已开启BBR
lsmod | grep bbr  #查看BBR是否启动
```

## how to use ssh to run shell script on a remote machine?

If Machine A is a Windows box, you can use Plink (part of PuTTY) with the -m parameter, and it will execute the local script on the remote server.

plink root@MachineB -m local_script.sh
If Machine A is a Unix-based system, you can use:
```bash
ssh root@MachineB 'bash -s' < local_script.sh
```

如果只需要执行单次命令: `ssh vbox "kcptun --version"`
