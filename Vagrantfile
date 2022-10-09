MACHINES = {
	:centos => {
		:box_name => "almalinux/8",
		:ip_addr => '192.168.1.2',
	},
}
 

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
	config.vm.define boxname do |box|
		config.vm.box = boxconfig[:box_name]
		box.vm.host_name = boxname.to_s
		box.vm.network "private_network", ip: boxconfig[:ip_addr], netmask: "255.255.255.0", tensor__intnet: "nfs_fuse_net"
		box.vm.provider :virtualbox do |vb|
		  vb.customize ["modifyvm", :id, "--memory", "512"]
		end
		box.vm.provision "shell", inline: <<-SHELL
 		  sudo useradd -p student student
		  SHELL
	end
  end
end
