# -*- mode: ruby -*-
# vi: set ft=ruby :
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!

# http://www.ollegustafsson.com/en/ultimate-mailserver-setup/#more-425
# https://gargamel.nu/my-mail-server-setup/
# https://github.com/modoboa/modoboa-installer/tree/master/modoboa_installer/scripts/files

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = "whmail-dev"
    config.vm.box_url = "https://github.com/holms/vagrant-jessie-box/releases/download/Jessie-v0.1/Debian-jessie-amd64-netboot.box"

    config.vm.network "public_network"
    #config.vm.network "forwarded_port", guest: 8080, host: 8080

    config.ssh.forward_agent = true
    config.vm.provider :virtualbox do |vb|
        # Don't boot with headless mode
        #vb.gui = true

        # Use VBoxManage to customize the VM. For example to change memory:
        vb.customize ["modifyvm", :id, "--memory", "800"]
        vb.customize ["modifyvm", :id, "--cpus", 2]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "all.yml"
        ansible.host_key_checking = false

        ansible.groups = {
            "webserver" => ["default"],
            "mailserver" => ["default"],
            "modoboa" => ["default"],
            "postgresql" => ["default"],
        }
        ansible.sudo = true
        #ansible.verbose = 'vvv'
        ansible.raw_arguments = ['-e pipelining=True']
    end

    if Vagrant.has_plugin?("vagrant-cachier")
        # Configure cached packages to be shared between instances of the same base box.
        config.cache.scope = :box
    end
end
