Vagrant.configure("2") do |config|

    config.vm.define "ansible-controller" do |controller|
        controller.vm.box = "ubuntu/jammy64"
        controller.vm.hostname = "ansible-controller"
        controller.vm.network "private_network", ip: "192.168.56.5"
        controller.vm.provider "virtualbox" do |vb|
          vb.memory = 1024
          vb.cpus = 1
      end
    
        # Compartilhar a pasta do projeto com a VM para facilitar a edição dentro dela
        controller.vm.synced_folder ".", "/home/vagrant/k8s-lab"
    end
  
    config.vm.define "k8s-master1" do |master1|
      master1.vm.box = "ubuntu/jammy64"
      master1.vm.hostname = "k8s-master1"
      master1.vm.network "private_network", ip: "192.168.56.10"
      master1.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
    end
  
    config.vm.define "k8s-master2" do |master2|
      master2.vm.box = "ubuntu/jammy64"  
      master2.vm.hostname = "k8s-master2"
      master2.vm.network "private_network", ip: "192.168.56.11"
      master2.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
    end
  
    config.vm.define "k8s-master3" do |master3|
      master3.vm.box = "ubuntu/jammy64"  
      master3.vm.hostname = "k8s-master3"
      master3.vm.network "private_network", ip: "192.168.56.12"
      master3.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
    end
  
    config.vm.define "k8s-worker1" do |worker1|
      worker1.vm.box = "ubuntu/jammy64"  
      worker1.vm.hostname = "k8s-worker1"
      worker1.vm.network "private_network", ip: "192.168.56.21"
      worker1.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end
    end
  
    config.vm.define "k8s-worker2" do |worker2|
      worker2.vm.box = "ubuntu/jammy64"   
      worker2.vm.hostname = "k8s-worker2"
      worker2.vm.network "private_network", ip: "192.168.56.22"
      worker2.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end
    end
  end
  