# Vagrant

## 配置网络
如果是主流的发行版, 可以通过 Vagrantfile 配置网络, 尝试了 openwrt 这类 linux, 发现不能配置网络.
```ruby
# findout interface names by: VBoxManage list bridgedifs
config.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)"

# Create a private network, which allows host-only access to the machine
# using a specific IP.
# config.vm.network "private_network", ip: "192.168.33.10"
config.vm.network "private_network", ip: "192.168.64.88"
```
