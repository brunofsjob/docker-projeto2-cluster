# -*- mode: ruby -*-
# vi: set ft=ruby  :

# definindo configuracao de hardware de cada maquina virtual e a imagem ubuntu 
machines = {
  "master" => {"memory" => "1024", "cpu" => "1", "ip" => "230", "image" => "bento/ubuntu-22.04"},
  "node01" => {"memory" => "1024", "cpu" => "1", "ip" => "231", "image" => "bento/ubuntu-22.04"},
  "node02" => {"memory" => "1024", "cpu" => "1", "ip" => "232", "image" => "bento/ubuntu-22.04"},
  "node03" => {"memory" => "1024", "cpu" => "1", "ip" => "233", "image" => "bento/ubuntu-22.04"}
}

Vagrant.configure("2") do |config|

  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.hostname = "#{name}"
      machine.vm.network "private_network", ip: "192.168.56.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        
        # aplicacao ajuste de configuracao de cada maquina virtual
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        
      end

      # define arquivo script docker.sh para instalar docker em cada maquina virtual
      machine.vm.provision "shell", path: "docker.sh"
      
      # se o nome da maquina virtual for master, executa script para definir como gerenciador dos demais nos
      if "#{name}" == "master"
        machine.vm.provision "shell", path: "master.sh"
      
      # executa script nos demais nos para acesso 
      else
        machine.vm.provision "shell", path: "worker.sh"
      end

    end
  end
end
