# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2010"

  config.vbguest.auto_update = false
  config.vm.network "private_network", type: "dhcp"
  config.vm.provision "shell", name: "certs",
                               inline: "cp /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/cacerts.txt"
  config.vm.provision "shell", name: "certs",
                               inline: "cp /etc/ssl/certs/ca-certificates.crt /usr/lib/python3/dist-packages/httplib2/cacerts.txt"
  config.vm.provision "shell", name: "certs",
                               inline: "cp /etc/ssl/certs/ca-certificates.crt /usr/lib/python3/dist-packages/certifi/cacert.pem"
#  config.vm.provision "shell", name: "DHCP",
#                               inline: "echo '[Network]\nDHCP=yes' >> /etc/systemd/network/eth0.network"
  config.vm.provision "shell", name: "restarting network",
                               inline: "systemctl restart networking"
  #config.vm.provision "shell", inline: "rm /etc/systemd/resolved.conf"
  #config.vm.provision "shell", inline: "systemctl restart systemd-resolved"
  #config.vm.provision "shell", inline: "curl -sfL https://direnv.net/install.sh | bash"
  #config.vm.provision "shell", inline: "echo 'eval \"$(direnv hook bash)\"' >> ~/.bashrc"
  config.vm.provision "shell", name: "resolved.conf",
                               inline: "sed -iback -e 's/^DNS=/#DNS=/' -e 's/^DNSSEC=yes/DNSSEC=false/' /etc/systemd/resolved.conf"
  config.vm.provision "shell", name: "restarting resolved",
                               inline: "systemctl restart systemd-resolved.service; sleep 45"
  config.vm.provision "shell", name: "adding ansible ppa",
                               inline: "apt-add-repository ppa:ansible/ansible -y"
  config.vm.provision "shell", name: "setting to focal",
                               inline: "sed -i.back -e 's/groovy/focal/' /etc/apt/sources.list.d/ansible-ubuntu-ansible-groovy.list"
  config.vm.provision "shell", name: "installing ansible",
                               inline: "apt-get update -y -qq && DEBIAN_FRONTEND=noninteractive apt-get install -y -qq ansible --option 'Dpkg::Options::=--force-confold'"
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.install = false
  end
end
