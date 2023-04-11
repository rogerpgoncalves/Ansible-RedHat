# -*- mode: ruby -*-
# vi: set ft=ruby :
vms = {
  "control-node" => {"memory" => "2024", "cpus" => 1, "ip" => "9", "image" => "generic/rocky8"},
  "web1" => {"memory" => "1024", "cpus" => 1, "ip" => "10", "image" => "generic/rocky8"},
  "db1" => {"memory" => "1024", "cpus" => 1, "ip" => "11", "image" => "generic/rocky8"},
  "develop" => {"memory" => "1024", "cpus" => 1, "ip" => "12", "image" => "centos/7"},
  "testing" => {"memory" => "1024", "cpus" => 1, "ip" => "13", "image" => "centos/7"},
  "monitoring" => {"memory" => "1024", "cpus" => 1, "ip" => "14", "image" => "centos/7"},
}

Vagrant.configure('2') do |config|

  config.vm.box_check_update = false
    vms.each do |name, conf|
    config.vm.define "#{name}" do |m|
      m.vm.box = "#{conf["image"]}"
      m.vm.hostname = "#{name}.rpgoncalves.net"
      m.vm.network 'private_network', ip: "172.25.250.#{conf['ip']}"
    config.vm.synced_folder ".", "/vagrant", type: "nfs", mount_options: ["vers=4,tcp"]

      m.vm.provider 'virtualbox' do |vb| # VirtualBox
        vb.memory = conf['memory']
        vb.cpus = conf['cpus']
        vb.name = conf['name']
      end
      m.vm.provider 'libvirt' do |lv| # Libvirt
        lv.memory = conf['memory']
        lv.cpus = conf['cpus']
        lv.cputopology :sockets => 1, :cores => conf['cpus'], :threads => '1'
      end

      m.vm.provision "shell", path: "provision.sh"
      m.vm.provision "shell", inline: <<-'SHELL'
      sudo yum -y install epel-release
      sudo yum update -y
      sudo yum install -y ansible vim nano
      #sudo apt update
      #sudo apt install ansible vim nano  
      #sudo mkdir -p /root/.ssh
      #sudo cp /vagrant/.ssh/id_rsa* /root/.ssh
      #sudo chmod 400 /root/.ssh/id_rsa*
      #sudo cp /vagrant/.ssh/id_rsa.pub /root/.ssh/authorized_keys
   
  	  cat << EOF >> /etc/hosts
      172.25.250.9 control-node.rpgoncalves.net control-node
      172.25.250.10 web1.rpgoncalves.net web1
      172.25.250.11 db1.rpgoncalves.net db1
      172.25.250.12 develop.rpgoncalves.net develop
      172.25.250.13 testing.rpgoncalves.net testing
      172.25.250.14 monitoring.rpgoncalves.net monitoring
  	  EOF
      SHELL
    end
  end
end
