$script_ansible = <<-SCRIPT
  apt update && \
  apt install -y ansible
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = "1"
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "./configs", "/configs"

  config.vm.define :control do |control|
    control.vm.network "private_network", ip: "192.168.33.10"

    control.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_practice_control"
    end

    control.vm.synced_folder "./ansible", "/ansible"

    control.vm.provision "shell",
      inline: "cp /configs/id_bionic .ssh/id_bionic"

    control.vm.provision "shell",
      inline: "chmod 600 .ssh/id_bionic"

    control.vm.provision "shell",
      inline: "chown vagrant:vagrant .ssh/id_bionic"

    control.vm.provision "shell", inline: $script_ansible
  end

  config.vm.define :node do |node|
    node.vm.network "forwarded_port", guest: 80, host: 8080
    node.vm.network "private_network", ip: "192.168.33.11"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_practice_node"
    end

    node.vm.provision "shell",
      inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
  end
end
