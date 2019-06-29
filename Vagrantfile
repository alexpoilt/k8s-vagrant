Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/cosmic64"
    config.vm.synced_folder "./shared", "/opt/vagrant/data"

    (1..3).each do |i|
        node_role = i == 1 ? "master" : "worker"
        node_name = "k8s-node-#{i}"
        ip_public = "192.168.0.#{200+i}"
        ip_private = "192.168.10.#{10+i}"

        config.vm.define node_name do |node|
            node.vm.hostname = node_name
            node.vm.network "public_network", ip: ip_public, bridge: "bridge0"
            node.vm.network "private_network", ip: ip_private

            node.vm.provider "virtualbox" do |vbox|
                vbox.name = node_name
                vbox.memory = 2048
                vbox.cpus = 2
            end

            node.vm.provision "shell" do |shell|
                shell.path = "./shared/bootstrap.sh"
                shell.args = [ip_private, node_role]
            end
        end
    end
end