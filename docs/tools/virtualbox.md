# Virtual Box

## Headless
`VBoxManage startvm $VMNAME --type headless`

or

` VBoxHeadless --startvm <uuid|name> & `

` VBoxManage controlvm <vmname> pause|resume|reset|poweroff|savestate `

## Network

guest 能上网，同时 host 无论连上网络与否都可以连接 guest：
首先在 virtualbox 的全局设置里开一个 host-only 网络，如此 host 多了一张虚拟网卡。guest 开两张网卡

1. 网卡1设为桥接，这样 guest 在网络里就像一个独立主机，可以上网
2. 网卡2设为 host-only ，
