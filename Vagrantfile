# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "geerlingguy/centos7"
  config.vm.box_version = "1.2.0"

  config.vm.hostname = "secrets-demo"

  config.vm.network :forwarded_port, guest: 8000, host: 8000,  auto_correct: true # consul-ui

  # Run Ansible from the host machine to provision the VM
  config.vm.provision "ansible" do |ansible|
    ansible.sudo              = true
    ansible.playbook          = "provision/playbook.yml"
    ansible.galaxy_role_file  = "requirements.yml"
  end
end
