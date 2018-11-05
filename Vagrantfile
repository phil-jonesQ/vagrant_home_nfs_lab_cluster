Vagrant.configure("2") do |config|

config.vm.define "nfs1" do |nfs1|

    nfs1.vm.box = "bento/centos-7.2"

    nfs1.vm.network "private_network", ip: "192.168.10.4"

    nfs1.vm.hostname = "nfs1"

    nfs1.vm.provider :virtualbox do |vb|

      vb.customize ["modifyvm", :id, "--memory", "1012"]

      vb.customize ["modifyvm", :id, "--cpus", "2"]

      end

    nfs1.vm.provision "shell", inline: <<-SHELL

      sudo echo "192.168.10.8 nfs2" | sudo tee -a /etc/hosts

      sudo echo "192.168.10.9 nfs3" | sudo tee -a /etc/hosts

      sudo systemctl enable firewalld

      sudo systemctl start firewalld

      sudo firewall-cmd --permanent --zone=public --add-port=8140/tcp

      sudo yum -y install ntp

      sudo timedatectl set-timezone Europe/London

      sudo systemctl start ntpd

      sudo firewall-cmd --add-service=ntp --permanent

    SHELL

  end

config.vm.define "nfs2" do |nfs2|

    nfs2.vm.box = "bento/centos-7.2"

    nfs2.vm.network "private_network", ip: "192.168.10.8"

    nfs2.vm.hostname = "puppetagent-1"

    nfs2.vm.provision "shell", inline: <<-SHELL

       sudo echo "192.168.10.4 nfs1" | sudo tee -a /etc/hosts

       sudo timedatectl set-timezone Europe/London

    SHELL

    end

 

    config.vm.define "nfs3" do |nfs3|

      nfs3.vm.box = "bento/centos-7.2"

      nfs3.vm.network "private_network", ip: "192.168.10.9"

      nfs3.vm.hostname = "puppetagent-2"

      nfs3.vm.provision "shell", inline: <<-SHELL

      sudo echo "192.168.10.4 nfs1" | sudo tee -a /etc/hosts

      sudo timedatectl set-timezone Europe/London

    SHELL

  end
end


