# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  
  config.vm.box = "ubuntu/bionic64"

  config.vm.define "control", primary: true do |h|
    h.vm.network "private_network", ip: "192.168.135.10"
    h.vm.hostname = "control"
    
    h.vm.provider "virtualbox" do |vb| 
      vb.memory = 2048
      vb.cpus = 2
    end
    
    h.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/control.pub

cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/

apt update -y
apt install ansible -y

sudo -- sh -c -e "echo '192.168.135.101 lb01\n192.168.135.111 app01\n192.168.135.112 app02\n192.168.135.121 db01' >> /etc/hosts"

if ! [ -L /etc/ansible/inventories ]; then
  ln -fs /vagrant/inventories /etc/ansible/inventories
fi


if ! [ -L /etc/ansible/playbooks ]; then
  ln -fs /vagrant/playbooks /etc/ansible/playbooks
fi

EOF
  end

  config.vm.define "lb01" do |h|
    h.vm.network "private_network", ip: "192.168.135.101"
    h.vm.hostname = "lb01"
    h.vm.network "forwarded_port", guest: 80, host: 8080

    h.vm.provider "virtualbox" do |vb| 
      vb.memory = 1048
      vb.cpus = 2
    end

    h.vm.provision :shell, :inline => <<'EOF'
cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys
apt install python -y
EOF
  end

  config.vm.define "app01" do |h|
    h.vm.network "private_network", ip: "192.168.135.111"
    h.vm.hostname = "app01"

    h.vm.provider "virtualbox" do |vb| 
      vb.memory = 1048
      vb.cpus = 2
    end

    h.vm.provision :shell, :inline => <<'EOF'
cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys
apt update -y
apt install python -y
EOF
  end

  config.vm.define "app02" do |h|
    h.vm.network "private_network", ip: "192.168.135.112"
    h.vm.hostname = "app02"

    h.vm.provider "virtualbox" do |vb| 
      vb.memory = 1048
      vb.cpus = 2
    end

    h.vm.provision :shell, :inline => <<'EOF'
cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys
apt update -y
apt install python -y
EOF
  end

  config.vm.define "db01" do |h|
    h.vm.network "private_network", ip: "192.168.135.121"
    h.vm.hostname = "db01"

    h.vm.provider "virtualbox" do |vb| 
      vb.memory = 1048
      vb.cpus = 2
    end

    h.vm.provision :shell, :inline => <<'EOF'
cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys
apt update -y
apt install python -y
EOF
  end
end