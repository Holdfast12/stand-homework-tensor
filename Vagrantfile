MACHINES = {
    :centos => {
        :box_name => "almalinux/8",
        :ip_addr => '192.168.1.2',
        :script => './server.sh',
    },
}
 

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
        config.vm.box = boxconfig[:box_name]
          config.vm.box_check_update = false
        box.vm.host_name = boxname.to_s
        box.vm.network "private_network", ip: boxconfig[:ip_addr], netmask: "255.255.255.0", virtualbox__intnet: "ansible_net"
        box.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--memory", "512"]
        end
        box.vm.provision "shell", inline: <<-SHELL
          echo -en "192.168.1.2 centos\n192.168.1.3 centos2\n\n" | sudo tee -a /etc/hosts
          sudo useradd student && echo -e "student\nstudent\n" | sudo passwd student
		  echo "student ALL=(ALL) NOPASSWD:ALL" | sudo tee -a /etc/sudoers
		  sudo sed -i 's:^PasswordAuthentication no:PasswordAuthentication yes:' /etc/ssh/sshd_config
		  sudo sed -i 's:^#PasswordAuthentication yes:PasswordAuthentication yes:' /etc/ssh/sshd_config
		  sudo systemctl reload sshd
          SHELL
        box.vm.provision "shell", path: boxconfig[:script]
    end
  end
end

# запуск плейбука
# ansible-playbook /vagrant/ansible/provision/playbook.yml