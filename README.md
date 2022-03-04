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