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