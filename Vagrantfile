# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.hostname = "smart-on-fhir"
  config.vm.box = "bento/ubuntu-16.04"
  #config.vm.box = "ubuntu/xenial64"
  #config.vm.box_url = "http://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-vagrant.box"
  #config.vm.box = "ubuntu/trusty64"
  #config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :forwarded_port, guest: 9080, host: 9080
  config.vm.network :forwarded_port, guest: 9085, host: 9085
  config.vm.network :forwarded_port, guest: 9090, host: 9090
#  config.vm.network :forwarded_port, guest: 9095, host: 9095

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.provision "fix-no-tty", type: "shell" do |s|
    s.privileged = false
    s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end

  config.vm.provision "shell", path: "provisioning/fetch-templates.sh", args: ["/vagrant/provisioning/roles/common/templates/config","v0.1.2"]

  config.vm.provision "ansible" do |ansible|
#    ansible.verbose="vvvv"
#    ansible.tags=["load_patients"]
    ansible.playbook = "provisioning/smart-on-fhir-servers.yml"
  end

end
