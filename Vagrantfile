Vagrant.configure("2") do |config|
   config.ssh.insert_key = false
  config.vm.define "ansible-controller" do |controller|
    controller.vm.box = "ubuntu/jammy64"
    controller.vm.hostname = "ansible-controller"
    controller.vm.network "private_network", ip: "192.168.56.5"

    # Sincroniza a pasta "key" do projeto para dentro da VM
    controller.vm.synced_folder "./key", "/vagrant_ssh"
  
    controller.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
     

# Provisionamento: copia chave privada para ~/.ssh/id_rsa e ajusta permissões
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
    
    # Compartilhar a pasta do projeto com a VM para facilitar a edição dentro dela
    controller.vm.synced_folder ".", "/home/vagrant/k8s-lab"
  
    # Instala Ansible automaticamente no provisionamento
    controller.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y software-properties-common
      sudo apt-add-repository --yes --update ppa:ansible/ansible
      sudo apt-get install -y ansible python3-pip

    SHELL
  
    # Cria usuário ansible com sudo sem senha
    #controller.vm.provision "shell", inline: <<-SHELL
      #sudo useradd -m -s /bin/bash ansible
      #echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/ansible
      #sudo mkdir -p /home/ansible/.ssh
      #sudo chown ansible:ansible /home/ansible/.ssh
      #sudo chmod 700 /home/ansible/.ssh
  
      # Gera chave SSH para o usuário vagra
      #sudo -u ansible ssh-keygen -t rsa -b 4096 -f /home/ansible/.ssh/id_rsa -N ""
      
      # Copia a chave pública para a pasta compartilhada
      #sudo mkdir -p /home/ansible/k8s-lab/keys
      #sudo cp /home/ansible/.ssh/id_rsa.pub /home/ansible/k8s-lab/keys/id_rsa.pub
      #sudo chown ansible:ansible /home/ansible/k8s-lab/keys/id_rsa.pub
    #SHELL
  end
  
  config.vm.define "k8s-master1" do |master1|
    master1.vm.box = "ubuntu/jammy64"
    master1.vm.hostname = "k8s-master1"
    master1.vm.network "private_network", ip: "192.168.56.10"

    ##master1.vm.synced_folder ".", "/home/ansible/k8s-lab"
  
    master1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  
    #master1.vm.provision "shell", inline: <<-SHELL
      #sudo useradd -m -s /bin/bash ansible
      #echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/ansible
      #sudo mkdir -p /home/ansible/.ssh
      #sudo cp /home/ansible/k8s-lab/keys/id_rsa.pub /home/ansible/.ssh/authorized_keys
      #sudo chown ansible:ansible /home/ansible/.ssh -R
     # sudo chmod 700 /home/ansible/.ssh
      #sudo chmod 600 /home/ansible/.ssh/authorized_keys
    #SHELL
  end
  
  config.vm.define "k8s-master2" do |master2|
    master2.vm.box = "ubuntu/jammy64"
    master2.vm.hostname = "k8s-master2"
    master2.vm.network "private_network", ip: "192.168.56.11"

   # master2.vm.synced_folder ".", "/home/ansible/k8s-lab"
  
    master2.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  
    #master2.vm.provision "shell", inline: <<-SHELL
     # sudo useradd -m -s /bin/bash ansible
     # echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/ansible
     # sudo mkdir -p /home/ansible/.ssh
     # sudo cp /home/ansible/k8s-lab/keys/id_rsa.pub /home/ansible/.ssh/authorized_keys
     # sudo chown ansible:ansible /home/ansible/.ssh -R
     # sudo chmod 700 /home/ansible/.ssh
     # sudo chmod 600 /home/ansible/.ssh/authorized_keys
    #SHELL
  end
  
  config.vm.define "k8s-master3" do |master3|
    master3.vm.box = "ubuntu/jammy64"
    master3.vm.hostname = "k8s-master3"
    master3.vm.network "private_network", ip: "192.168.56.12"
    
  #  master3.vm.synced_folder ".", "/home/ansible/k8s-lab"
  
    master3.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  
   # master3.vm.provision "shell", inline: <<-SHELL
     # sudo useradd -m -s /bin/bash ansible
    #  echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/ansible
      #sudo mkdir -p /home/ansible/.ssh
      #sudo cp /home/ansible/k8s-lab/keys/id_rsa.pub /home/ansible/.ssh/authorized_keys
      #sudo chown ansible:ansible /home/ansible/.ssh -R
      #sudo chmod 700 /home/ansible/.ssh
      #sudo chmod 600 /home/ansible/.ssh/authorized_keys
    #SHELL
  end
  
  config.vm.define "k8s-worker1" do |worker1|
    worker1.vm.box = "ubuntu/jammy64"
    worker1.vm.hostname = "k8s-worker1"
    worker1.vm.network "private_network", ip: "192.168.56.21"

    #worker1.vm.synced_folder ".", "/home/ansible/k8s-lab"
  
    worker1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  
    #worker1.vm.provision "shell", inline: <<-SHELL
    #  sudo useradd -m -s /bin/bash ansible
    #  echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/ansible
    #  sudo mkdir -p /home/ansible/.ssh
    #  sudo cp /home/ansible/k8s-lab/keys/id_rsa.pub /home/ansible/.ssh/authorized_keys
    #  sudo chown ansible:ansible /home/ansible/.ssh -R
    # sudo chmod 700 /home/ansible/.ssh
    # sudo chmod 600 /home/ansible/.ssh/authorized_keys
    #HELL
  end
  
  config.vm.define "k8s-worker2" do |worker2|
    worker2.vm.box = "ubuntu/jammy64"
    worker2.vm.hostname = "k8s-worker2"
    worker2.vm.network "private_network", ip: "192.168.56.22"
    
    #orker2.vm.synced_folder ".", "/home/ansible/k8s-lab"
    
    worker2.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  
    #orker2.vm.provision "shell", inline: <<-SHELL
     # sudo useradd -m -s /bin/bash ansible
     # echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/ansible
     # sudo mkdir -p /home/ansible/.ssh
     # sudo cp /home/ansible/k8s-lab/keys/id_rsa.pub /home/ansible/.ssh/authorized_keys
     # sudo chown ansible:ansible /home/ansible/.ssh -R
     # sudo chmod 700 /home/ansible/.ssh
     # sudo chmod 600 /home/ansible/.ssh/authorized_keys
    #SHELL
  end
end
