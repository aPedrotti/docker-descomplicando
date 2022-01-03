# Instalation of a SWARM cluster 1 MANAGER and 2 WORKERS

$install_docker_script = <<SCRIPT
echo Installing Docker...
curl -sSL https://get.docker.com/ | sh
sudo usermod -aG docker vagrant
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
SCRIPT
$manager_script = <<SCRIPT
echo Swarm Init...
sudo docker swarm init --listen-addr 10.10.10.10:2377 --advertise-addr 10.10.10.10:2377
sudo docker swarm join-token --quiet worker > /home/vagrant/docker/worker_token
apt-get install nfs-server -y
SCRIPT
$worker_script = <<SCRIPT
echo Swarm Join...
sudo docker swarm join --token $(cat /home/vagrant/docker/worker_token) 10.10.10.10:2377
apt-get install nfs-common -y
SCRIPT
$aliases = <<SCRIPT
echo "alias d='docker'" >> /home/vagrant/.bashrc
echo "alias de='docker exec -it'" >> /home/vagrant/.bashrc
echo "alias ds='docker ps'" >> /home/vagrant/.bashrc
echo "alias dsv='docker service'" >> /home/vagrant/.bashrc
echo "alias dsw='docker swarm'" >> /home/vagrant/.bashrc
echo "alias dn='docker node'" >> /home/vagrant/.bashrc
echo "alias dc='docker-compose'" >> /home/vagrant/.bashrc
SCRIPT

Vagrant.configure('2') do |config|
    vm_box = 'ubuntu/bionic64'
    config.vm.define :manager, primary: true  do |manager|
        manager.vm.box = vm_box
        manager.vm.box_check_update = true
        manager.vm.hostname = "manager" 
        manager.vm.network :private_network, ip: "10.10.10.10", hostname: true
        for n in 8080..8089
            manager.vm.network :forwarded_port, guest: n , host: n
        end
        for n in 5000..5010
            manager.vm.network :forwarded_port, guest: n , host: n
        end
        manager.vm.network :forwarded_port, guest: 3000 , host: 3000
        manager.vm.network :forwarded_port, guest: 3306 , host: 3306
        manager.vm.network :forwarded_port, guest: 5432 , host: 5432
        manager.vm.network :forwarded_port, guest: 6379 , host: 6379
        manager.vm.synced_folder ".", "/home/vagrant/docker"
        manager.vm.provision "shell", inline: $install_docker_script, privileged: true
        manager.vm.provision "shell", inline: $manager_script, privileged: true
        manager.vm.provision "shell", inline: $aliases
        manager.vm.provider "virtualbox" do |vb|
            vb.name = "manager"
            vb.memory = "1024"
        end
    end
    (1..2).each do |i| # Pending check how to run them in paralel
        config.vm.define "worker0#{i}" do |worker|
            worker.vm.box = vm_box
            worker.vm.box_check_update = true
            worker.vm.hostname = "worker0#{i}"
            worker.vm.network :private_network, ip: "10.10.10.1#{i}"
            worker.vm.synced_folder "./", "/home/vagrant/docker"
            worker.vm.provision "shell", inline: $install_docker_script, privileged: true
            worker.vm.provision "shell", inline: $worker_script, privileged: true
            worker.vm.provision "shell", inline: $aliases
            worker.vm.provider "virtualbox" do |vb|
                vb.name = "worker0#{i}"
                vb.memory = "1024"
            end
        end
    end
end