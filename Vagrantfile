# -*- mode: ruby -*-
# vi: set ft=ruby 

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = "centos/7"
    config.vm.box_check_update = false
    config.vm.hostname = "vagrantbox"

    config.vm.network :forwarded_port, host: 80, guest: 80, auto_correct: true # website
    config.vm.network :forwarded_port, guest: 443, host: 443, auto_correct: true # ssl

    config.vm.define :node1 do |node|
      node.vm.box = "centos/7"
      node.vm.network "private_network", ip: "170.11.11.11"
    end

    config.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "512"
    end

    config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
      s.inline = <<-SHELL
      echo "#{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys"
      SHELL
  end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.sudo = true
        #ansible.inventory_path = "playbooks"
    end

    config.vm.provision :shell, inline: "echo Good job"

end
