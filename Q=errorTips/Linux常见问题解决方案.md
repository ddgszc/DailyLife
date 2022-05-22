# Linux常见问题解决

[toc]


### Linux文件系统架构

- /bin 二进制文件
- /boot 系统启动分区，系统启动时读取的文件
- /dev 设备文件
- /etc 大多数配置文件
- /home 用户目录
- /lib 32位函数库
- /lib64 64位库
- /media 手动临时挂载点
- /mnt 手动临时挂载点
- /opt 第三方软件安装位置
- /proc 进程信息及硬件信息
- /root 临时设备的默认挂载点
- /sbin 系统管理命令
- /srv 数据
- /var 数据
- /sys 内核相关信息
- /tmp 临时文件
- /usr 用户相关设定

### ssh登录

- Ubuntu开启SSH服务

  1. 安装ssh服务：apt install openssh-server

  2. 开启ssh服务：service start ssh

  3. 查看是否启动ssh服务：ps -e | grep ssh  --> 为空就是没有开启

  4. **允许root用户登录**：修改/etc/ssh/sshd_config 文件

     Port 22 --> Port 22

     PermitRootLogin prohibit-password --> PermitRootLogin yes

     重启sshd服务：service sshd restart

- 固定IP地址

  1. 配NAT模式IP地址：

     a.虚拟网络编辑器,NAT设置选项里面的网关192.168.36.1(任意选择)

     b.子网IP 192.168.36.0 ，子网掩码：255.255.255.0

     c.配置VMnet8网卡(NAT)的IP地址，设置好网关：192.168.36.1

  2. 设置固定IP地址

     a.Ubuntu从17.10开始，不在/etc/network/interfaces配置IP地址，转到netplan方式，

     b.备份：mv /etc/netplan/01-network-manager-all.yaml ./01-network-manager-all.yaml.bak

     c.修改01-network-manager-all.yaml内容为：

     ```shell
     # Let NetworkManager manage all devices on this system
     network:
       version: 2
       renderer: NetworkManager
       ethernets:
         ens33:   # 网卡名称
           dhcp4: no     # 关闭dhcp
           dhcp6: no
           addresses: [192.168.36.13001  /24]  # 静态ip
           routes: 
           - to: default
             via: 192.168.36.1     # 网关
           nameservers:
             addresses: [192.168.36.1] #dns
     ```

     d.sudo netplan apply

- VScode免密登录

  1.terminal执行**ssh-keygen**生成公私钥，(可以指定密钥文件名)

  2.将公钥id_rsa.pub的内容复制到远程主机~/.ssh/authorized_keys

  3.配置VScode配置文件内容

  ```shell
  Host Ubuntu22
      HostName 192.168.36.129
      User root
      IdentityFile "C:\Users\Admin\.ssh\id_rsa"
  ```



### 关机

- 重启：shutdown -r now | reboot
- shutdown -r 2 //延迟两分钟重启
- 关机：shutdown -h now | poweroff
- shutdown -h 2 //延迟两分钟关机





### 查看进程

ps -ef 查看进程 ps-ef | grep 程序名

kill -9 [pid] 杀死指定程序进程























