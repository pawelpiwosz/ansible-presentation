Vagrant.configure(2) do |config|
    config.vm.synced_folder "dataNode/", "/dataNode"

    config.vm.define "ansiblehost" do |ansiblehost|
        ansiblehost.vm.box = "ubuntu/bionic64"
        ansiblehost.vm.hostname = "ansiblehost"
        ansiblehost.vm.network "forwarded_port", guest: 22, host: 10188
        ansiblehost.vm.network "private_network", ip: "10.100.198.188"
        ansiblehost.vm.provider "virtualbox" do |v|
            v.memory = 2048
        end
    end
    config.vm.define "ansiblenode" do |ansiblenode|
        ansiblenode.vm.box = "ubuntu/bionic64"
        ansiblenode.vm.hostname = "ansiblenode"
        ansiblenode.vm.network "forwarded_port", guest: 80, host: 8080
        ansiblenode.vm.network "forwarded_port", guest: 22, host: 10189
        ansiblenode.vm.network "private_network", ip: "10.100.198.189"
        ansiblenode.vm.provider "virtualbox" do |v|
            v.memory = 2048
        end
    end

end
