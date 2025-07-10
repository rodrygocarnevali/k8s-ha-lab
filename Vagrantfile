    # Define a versão da configuração do Vagrant
Vagrant.configure("2") do |config|

    # Impede a inserção automática de chaves SSH no processo de inicialização
   config.ssh.insert_key = false

  # ================================
  # NÓ CONTROLADOR (Ansible)
  # ================================   
  config.vm.define "ansible-controller" do |controller|
    controller.vm.box = "ubuntu/jammy64"
    controller.vm.hostname = "ansible-controller"
    controller.vm.network "private_network", ip: "192.168.56.5"

    # Monta uma pasta local para a VM com chaves SSH personalizadas
    controller.vm.synced_folder "./key", "/vagrant_ssh"

  # Configuração de recursos da VM (memória e CPU)  
    controller.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end

# Primeiro provisionamento: atualização e sincronização de horário
    controller.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y ntpdate
    ntpdate time.google.com
    SHELL
    
 # Segundo provisionamento: cópia da chave SSH para acesso a outros nós
    controller.vm.provision "shell", inline: <<-SHELL
    echo "Esperando a pasta /vagrant_ssh estar montada..."
    while [ ! -f /vagrant_ssh/insecure_private_key ]; do sleep 1; done
    echo "Pasta montada, copiando a chave..."
    mkdir -p /home/vagrant/.ssh
    cp /vagrant_ssh/insecure_private_key /home/vagrant/.ssh/id_rsa
    chmod 600 /home/vagrant/.ssh/id_rsa
    chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
    echo "Concluído."

      # Desativa confirmação de host SSH
    echo "Host *" > /home/vagrant/.ssh/config
    echo "   StrictHostKeyChecking no" >> /home/vagrant/.ssh/config
    echo "   UserKnownHostsFile=/dev/null" >> /home/vagrant/.ssh/config
    chmod 600 /home/vagrant/.ssh/config
    chown vagrant:vagrant /home/vagrant/.ssh/config
    echo "Provisionamento concluído."
    
    SHELL
    
     controller.vm.synced_folder ".", "/home/vagrant/k8s-lab"
  
      controller.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y software-properties-common
      sudo apt-add-repository --yes --update ppa:ansible/ansible
      sudo apt-get install -y ansible python3-pip

    SHELL
  
  end
  
  config.vm.define "k8s-master1" do |master1|
    master1.vm.box = "ubuntu/jammy64"
    master1.vm.hostname = "k8s-master1"
    master1.vm.network "private_network", ip: "192.168.56.10"
   
    master1.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
      vb.customize ["modifyvm", :id, "--nested-hw-virt","on"]
      vb.customize ["modifyvm", :id, "--cpuhotplug","on"]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype3", "virtio"]
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]

    end
  

     master1.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y ntpdate
    ntpdate time.google.com
    SHELL

  end
  
  config.vm.define "k8s-master2" do |master2|
    master2.vm.box = "ubuntu/jammy64"
    master2.vm.hostname = "k8s-master2"
    master2.vm.network "private_network", ip: "192.168.56.11"


  
    master2.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
      vb.customize ["modifyvm", :id, "--nested-hw-virt","on"]
      vb.customize ["modifyvm", :id, "--cpuhotplug","on"]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype3", "virtio"]
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    end


    master2.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y ntpdate
    ntpdate time.google.com
    SHELL

  end
  
  config.vm.define "k8s-master3" do |master3|
    master3.vm.box = "ubuntu/jammy64"
    master3.vm.hostname = "k8s-master3"
    master3.vm.network "private_network", ip: "192.168.56.12"
    
    master3.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
      vb.customize ["modifyvm", :id, "--nested-hw-virt","on"]
      vb.customize ["modifyvm", :id, "--cpuhotplug","on"]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype3", "virtio"]
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    end
  
    master3.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y ntpdate
    ntpdate time.google.com
    SHELL

  end
  
  config.vm.define "k8s-worker1" do |worker1|
    worker1.vm.box = "ubuntu/jammy64"
    worker1.vm.hostname = "k8s-worker1"
    worker1.vm.network "private_network", ip: "192.168.56.21"

  
    worker1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
      vb.customize ["modifyvm", :id, "--nested-hw-virt","on"]
      vb.customize ["modifyvm", :id, "--cpuhotplug","on"]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype3", "virtio"]
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    end
  

     worker1.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y ntpdate
    ntpdate time.google.com
    SHELL


  end
  
  config.vm.define "k8s-worker2" do |worker2|
    worker2.vm.box = "ubuntu/jammy64"
    worker2.vm.hostname = "k8s-worker2"
    worker2.vm.network "private_network", ip: "192.168.56.22"
    
     
    worker2.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
      vb.customize ["modifyvm", :id, "--nested-hw-virt","on"]
      vb.customize ["modifyvm", :id, "--cpuhotplug","on"]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
      vb.customize ["modifyvm", :id, "--nictype3", "virtio"]
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    end
  
    worker2.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y ntpdate
    ntpdate time.google.com
    SHELL

  end
end