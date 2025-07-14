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
    controller.vm.synced_folder "C:/Users/#{ENV['USERNAME']}/.vagrant.d", "/vagrant_ssh", type: "virtualbox"

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

      # Configura o cliente SSH para ignorar verificação de host
      echo "Host *" > /home/vagrant/.ssh/config
      echo "   StrictHostKeyChecking no" >> /home/vagrant/.ssh/config
      echo "   UserKnownHostsFile=/dev/null" >> /home/vagrant/.ssh/config
      chmod 600 /home/vagrant/.ssh/config
      chown vagrant:vagrant /home/vagrant/.ssh/config
      echo "Provisionamento concluído."
    SHELL

    # Monta o diretório do projeto local dentro da VM
    controller.vm.synced_folder ".", "/home/vagrant/k8s-lab"

    # Terceiro provisionamento: instalação do Ansible e dependências
    controller.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y software-properties-common
      sudo apt-add-repository --yes --update ppa:ansible/ansible
      sudo apt-get install -y ansible python3-pip
    SHELL
  end

  # ================================
  # MASTERS K8s (1)
  # ================================
  config.vm.define "k8s-master1" do |master1|
    master1.vm.box = "ubuntu/jammy64"
    master1.vm.hostname = "k8s-master1"
    master1.vm.network "private_network", ip: "192.168.56.10"
   
    # Recursos e configurações avançadas da VM
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

    # Atualização e sincronização de horário
    master1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ntpdate
      ntpdate time.google.com
    SHELL
  end

  # ================================
  # MASTERS K8s (2)
  # ================================
  config.vm.define "k8s-master2" do |master2|
    master2.vm.box = "ubuntu/jammy64"
    master2.vm.hostname = "k8s-master2"
    master2.vm.network "private_network", ip: "192.168.56.11"

    # Recursos e configurações avançadas da VM
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

    # Atualização e sincronização de horário
    master2.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ntpdate
      ntpdate time.google.com
    SHELL
  end

  # ================================
  # MASTERS K8s (3)
  # ================================
  config.vm.define "k8s-master3" do |master3|
    master3.vm.box = "ubuntu/jammy64"
    master3.vm.hostname = "k8s-master3"
    master3.vm.network "private_network", ip: "192.168.56.12"

    # Recursos e configurações avançadas da VM
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

    # Atualização e sincronização de horário
    master3.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ntpdate
      ntpdate time.google.com
    SHELL
  end

  # ================================
  # worker K8s (1)
  # ================================
  config.vm.define "k8s-worker1" do |worker1|
    worker1.vm.box = "ubuntu/jammy64"
    worker1.vm.hostname = "k8s-worker1"
    worker1.vm.network "private_network", ip: "192.168.56.21"

    # Recursos e configurações avançadas da VM
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

    # Atualização e sincronização de horário
    worker1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ntpdate
      ntpdate time.google.com
    SHELL
  end

  # ================================
  # worker K8s (2)
  # ================================
  config.vm.define "k8s-worker2" do |worker2|
    worker2.vm.box = "ubuntu/jammy64"
    worker2.vm.hostname = "k8s-worker2"
    worker2.vm.network "private_network", ip: "192.168.56.22"

    # Recursos e configurações avançadas da VM  
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

    # Atualização e sincronização de horário
    worker2.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ntpdate
      ntpdate time.google.com
    SHELL
  end

  # ================================
  # LoadBalancer HA
  # ================================
  config.vm.define "k8s-ha" do |ha|
    ha.vm.box = "ubuntu/jammy64"
    ha.vm.hostname = "k8s-ha"
    ha.vm.network "private_network", ip: "192.168.56.13"

    # Recursos e configurações avançadas da VM  
    ha.vm.provider "virtualbox" do |vb|
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

    # Atualização e sincronização de horário
    ha.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ntpdate
      ntpdate time.google.com
    SHELL
  end
end
