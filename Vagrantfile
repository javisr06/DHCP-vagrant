Vagrant.configure("2") do |config|
  config.vm.define "server" do |srv|
    srv.vm.box = "debian/bullseye64"
    
    srv.vm.network "public_network", bridge: "enp4s0"
    
    # Adaptador de red interna
    srv.vm.network "private_network",
      ip: "192.168.57.10",
      virtualbox__intnet: "intnet"
  end
end