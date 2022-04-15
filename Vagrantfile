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

  config.vm.define :wordpress do |wordpress|
    wordpress.vm.network "forwarded_port", guest: 80, host: 8080
    wordpress.vm.network "private_network", ip: "192.168.33.11"

    wordpress.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_practice_wordpress"
    end

    wordpress.vm.provision "shell",
      inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
  end

  config.vm.define :mysql do |mysql|
    mysql.vm.network "forwarded_port", guest: 3306, host: 3306
    mysql.vm.network "private_network", ip: "192.168.33.12"

    mysql.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_practice_mysql"
    end

    mysql.vm.provision "shell",
      inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
  end

  config.vm.define :docker do |docker|
    docker.vm.network "forwarded_port", guest: 3306, host: 3306
    docker.vm.network "private_network", ip: "192.168.33.13"

    docker.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_practice_docker"
    end

    docker.vm.provision "shell",
      inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
  end
end
