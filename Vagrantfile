Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/trusty64"
    config.vm.hostname = "bm.env-test"

    config.vm.define :test_bmenv do |test_bmenv|
        test_bmenv.vm.network :private_network, ip: "192.168.56.101"
        test_bmenv.vm.network :forwarded_port, guest: 80, host: 8091
        
        if Vagrant::Util::Platform.windows?
            test_bmenv.vm.synced_folder "./../src/", "/var/www"
        else
            test_bmenv.vm.synced_folder "./www", "/var/www", type: "nfs"
        end

        test_bmenv.ssh.forward_agent = true

        test_bmenv.vm.provider :virtualbox do |v|
            v.name = "storage"
            v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
            v.customize ["modifyvm", :id, "--memory", "2096"]
            v.customize ["modifyvm", :id, "--cpus", "2"]
            v.customize ["modifyvm", :id, "--ioapic", "on"]
            v.customize ["modifyvm", :id, "--name", "bm-env-box"]
        end

        test_bmenv.vbguest.auto_update = true

        test_bmenv.vm.provision :shell, :inline => "sudo apt-get update --fix-missing"
    end
end
