IMAGE_NAME = "ubuntu/focal64"
N = 3

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end

    config.vm.define "bastion" do |bastion|
        bastion.vm.box = IMAGE_NAME
        bastion.vm.network "private_network", ip: "10.10.1.3"
        bastion.vm.hostname = "bastion"
        bastion.vm.synced_folder "./ansible", "/ansible", type: "smb"

        bastion.vm.provision "ansible" do |ansible|
            ansible.playbook = "bastion.yml"
            ansible.extra_vars = {
                node_ip: "10.10.1.3",
                ansible_python_interpreter: "/usr/bin/python3",
            }
        end
    end
    
#    config.vm.provision "shell", inline: <<-SHELL
#        wget -q https://github.com/lucasvictor477/MaxMilhas/blob/6c18684d2b711f81175fbdb7fadc7bb4c8f4755f/index.html -O /var/www/html/index.html;
#        service apache2 restart;
#    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "10.10.1.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "node.yml"
                ansible.extra_vars = {
                    node_ip: "10.10.1.#{i + 10}",
                    ansible_python_interpreter: "/usr/bin/python3",
                }
            end
        end
    end
end
