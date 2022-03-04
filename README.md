# Vagrant

In this repo, I am going to experiment with `vagrant`.

## Devbox : Mac

```bash
sw_vers
ProductName:	macOS
ProductVersion:	12.1
BuildVersion:	21C52
```

## Version `vagrant --version`

```bash
Vagrant 2.2.19
```
## Steps

```bash
vagrant init ubuntu/trusty64
```

It creates the template vagrant file that I can edit for my use

## Deploying Nginx

Add the following towards the end before the `end` statement

```yaml
  config.vm.network "forwarded_port", guest: 80, host: 9191
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx
  SHELL
```

## logs

```bash
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu/trusty64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/trusty64' version '20190514.0.0' is up to date...
==> default: Setting the name of the VM: nginx_default_1646384843832_22553
==> default: Clearing any previously set forwarded ports...
==> default: Fixed port collision for 22 => 2222. Now on port 2200.
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 80 (guest) => 9191 (host) (adapter 1)
    default: 22 (guest) => 2200 (host) (adapter 1)
```

## Voila!

~[](./images/localhost.png)

## Port forwarding

```yaml
config.vm.network "forwarded_port", guest: 80, host: 9191
```

## PRIVATE IP

```yaml
  config.vm.network "private_network", type: "dhcp"
```

Reload the box

### vagrant reload --provision

```bash
vagrant reload --provision
==> default: Attempting graceful shutdown of VM...
==> default: Checking if box 'ubuntu/trusty64' version '20190514.0.0' is up to date...
==> default: Clearing any previously set forwarded ports...
==> default: Fixed port collision for 22 => 2222. Now on port 2200.
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 80 (guest) => 9191 (host) (adapter 1)
    default: 22 (guest) => 2200 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2200
    default: SSH username: vagrant
    default: SSH auth method: private key
==> default: Machine booted and ready!
[default] GuestAdditions 6.1.30 running --- OK.
==> default: Checking for guest additions in VM...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /vagrant => /Users/ss670121/sourcebox/brcm.github.com/test/nginx
==> default: Running provisioner: shell...
```

### Check the IP of the machine

```bash
vagrant ssh
```

```bash
vagrant@vagrant-ubuntu-trusty-64:~$ ifconfig
eth0      Link encap:Ethernet  HWaddr 08:00:27:5f:bb:e6  
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe5f:bbe6/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1152 errors:0 dropped:0 overruns:0 frame:0
          TX packets:866 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:144881 (144.8 KB)  TX bytes:158024 (158.0 KB)

eth1      Link encap:Ethernet  HWaddr 08:00:27:94:d2:d4  
          inet addr:192.168.56.7  Bcast:192.168.56.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe94:d2d4/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:13 errors:0 dropped:0 overruns:0 frame:0
          TX packets:5 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:5602 (5.6 KB)  TX bytes:942 (942.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```


![](images/ip.png)

## Static IP

```yaml
config.vm.network "private_network", ip: "192.168.56.11"
```

```bash
vagrant reload --provision
```

![](./images/static-ip.png)