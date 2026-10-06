# Servidor DHCP
Vagrant.configure("2") do |config|
  config.vm.define "server" do |srv|
    srv.vm.box = "debian/bullseye64"

    srv.vm.network "public_network",
      bridge: "Realtek PCIe GbE Family Controller"

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

  # Impresora
  config.vm.define "printer" do |printer|
    printer.vm.box = "debian/bullseye64"

    printer.vm.network "private_network",
      mac: "080027123456",
      type: "dhcp",
      virtualbox__intnet: "intnet"
  end
end