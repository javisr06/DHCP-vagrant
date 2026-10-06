```
mkdir DHCP-Vagrant
cd DHCP-Vagrant
git commit --allow-empty -m "Initial commit:status DHCP practice repository"
mkdir doc
touch doc/README.md
ip add
    2: enp4s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 9c:6b:00:0b:fa:0e brd ff:ff:ff:ff:ff:ff
    inet 10.209.10.1/16 brd 10.209.255.255 scope global noprefixroute enp4s0
       valid_lft forever preferred_lft forever
    inet6 fe80::730d:5c3f:b231:837c/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
``` 
vagrant up
```
``` 
sudo apt install isc-dhcp-server
```
```
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak
```
```
sudo nano /etc/dhcp/dhcpd.conf
```
```
default-lease-time 86400;
max-lease-time 691200;
option domain-name "tu_nombre.test";
option domain-name-servers 10.0.0.2, 4.4.4.4;

subnet 192.168.57.0 netmask 255.255.255.0 {
    range 192.168.57.20 192.168.57.50;
    option routers 192.168.57.10;
    option subnet-mask 255.255.255.0;
}
```[cite: 4]
```
nano Vagrantfile
```
```
#Servidor DHCP
Vagrant.configure("2") do |config|
  config.vm.define "server" do |srv|
    srv.vm.box = "debian/bullseye64"
    
    srv.vm.network "public_network", bridge: "enp4s0"
    
    # Adaptador de red interna
    srv.vm.network "private_network",
      ip: "192.168.57.10",
      virtualbox__intnet: "intnet"
  end
   # Cliente DHCP
  config.vm.define "client" do |cli|
  cli.vm.box = "debian/bullseye64"

  cli.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--nic2", "intnet"]
  end
end
end
```
```
vagrant reload client
```
