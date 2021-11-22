IMAGE_NAME = "ubuntu/focal64"
N = 3

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end

    config.vm.define "vm01" do |vm03|
        vm03.vm.box = IMAGE_NAME
        vm03.vm.network "private_network", ip: "10.10.1.3"
        vm03.vm.hostname = "vm03"
        vm03.vm.synced_folder "./ansible", "/ansible", type: "nfs"

        vm03.vm.provision "ansible" do |ansible|
            ansible.playbook = "vm03.yml"
            ansible.extra_vars = {
                node_ip: "10.10.1.3",
                ansible_python_interpreter: "/usr/bin/python3",
            }
        end
    end

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
