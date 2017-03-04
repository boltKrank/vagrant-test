# -*- mode: ruby -*-
# vi: set ft=ruby :

domain = "test.internal"
master = "puppet"
base_ip = "192.168.13."
pm_ip = base_ip + "10"
nodes = [
  "agent1.#{domain}",
  "agent2.#{domain}"
]

#VM Config:
Vagrant.configure("2") do |config|
  config.vm.box = "puppetlabs/centos-7.2-64-nocm"

  #pass the following commands to the VM's shell
  config.vm.provision "shell", inline: <<-SHELL

  mkdir -p /etc/facter/facts.d
  echo "vagrant=true" > /etc/facter/facts.d/vagrant.txt

  # install handy tools
  yum install -y wget links

  # restart network or our VM will be unreachable :(
  systemctl restart network

  # generate SSH keypair if we need to
  mkdir -p /vagrant/ssh_keys
  mkdir -p /root/.ssh
  chmod 700 /root/.ssh
  if [ ! -f /vagrant/ssh_keys/id_rsa ] ; then
    ssh-keygen -f /vagrant/ssh_keys/id_rsa -N ''
  fi
  # pick up existing generated key and use allow it to login.
  # we used to have a different key for each host but if you go
  # over 5 hosts the ssh-agent will get a failure on some hosts
  # because we run out of trys before we find the right key(!) so
  # just use the one keypair...
  cat /vagrant/ssh_keys/id_rsa.pub >> /root/.ssh/authorized_keys
  chmod 600 /root/.ssh/authorized_keys
  # for convenience, copy back the keypair to enable root to ssh to
  # localhost - this is good for things like testing out ssh+git
  cp /vagrant/ssh_keys/id_rsa* /root/.ssh/
  # firewall OFF (we will install iptables later...)
  systemctl disable firewalld
  systemctl stop firewalld

  SHELL



  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
