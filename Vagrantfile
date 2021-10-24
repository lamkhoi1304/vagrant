Vagrant.configure("2") do |config|
    servers=[
        {
            :hostname => "server1",
            :box => "bento/ubuntu-20.04",
            :ip => "172.16.1.50",
            :ssh_port => '2200'
        },
        {
            :hostname => "server2",
            :box => "bento/ubuntu-20.04",
            :ip => "172.16.1.51",
            :ssh_port => '2201'
        },
        {
            :hostname => "server3",
            :box => "bento/ubuntu-20.04",
            :ip => "172.16.1.52",
            :ssh_port => '2202'
        }
    ]

    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]

            node.vm.network :private_network, ip: machine[:ip]
            node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"

            node.vm.synced_folder "./data", "/home/vagrant/data"
            node.vm.provision "file", source: "hello.txt", destination: "hello.txt"

            node.vm.provider :virtualbox do |vb|
                vb.customize ["modifyvm", :id, "--memory", 512]
                vb.customize ["modifyvm", :id, "--cpus", 1]
                vb.customize ["modifyvm", :id, "--name", machine[:hostname]]
            end
        end
    end
end