# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  #Following is Vagrant definition for Monitoring VM
  config.vm.define "netmon" do |netmon|
    netmon.vm.box = "centos/7"

    netmon.vm.network "forwarded_port", guest: 9090, host: 9090, host_ip: "127.0.0.1"
    netmon.vm.network 'private_network', virtualbox__intnet: true,
      ip: '192.168.111.10'
    netmon.vm.provider :virtualbox do |vb|
      vb.customize ['modifyvm', :id, '--nic2', 'intnet',
                    '--intnet2', 'mgmt']
    end

    netmon.vm.provision "ansible" do |ansible|
      ansible.playbook = "provision/prometheus.yml"
    end
  end

  config.vm.define "eos" do |eos|
    eos.vm.box = "vEOS-lab-4.17.5M"

    # Create Ethernet1
    eos.vm.network 'private_network', virtualbox__intnet: true,
                   ip: '169.254.1.11', auto_config: false

    # Create Ethernet2
    eos.vm.network 'private_network', virtualbox__intnet: true,
                   ip: '169.254.1.11', auto_config: false

    eos.vm.provision 'shell', inline: <<-SHELL
     /usr/bin/FastCli -p 15 -c "configure
snmp-server community public ro
management api http-commands
ip routing
interface Ethernet1
  no shutdown
  no switchport
  ip address 192.168.111.20/24
end
copy running-config startup-config"
SHELL

    eos.vm.provider :virtualbox do |vb|
      # Networking:
      # nic1 is always Management1 which is set to dhcp in the basebox.
      # provisioning/ssh will fail otherwise
      #
      # Patch Ethernet1 to a particular internal network
      vb.customize ['modifyvm', :id, '--nic2', 'intnet',
                    '--intnet2', 'mgmt']
      # Patch Ethernet2 to a particular internal network
      vb.customize ['modifyvm', :id, '--nic3', 'intnet',
                    '--intnet3', 'vEOS-intnet2']
      # Display the VirtualBox GUI when booting the machine
      # Feel free to set to "false" if you don't need it
      vb.gui = true
    end
  end
end

