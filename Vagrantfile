# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # Number of hslave nodes
  num_hslave_nodes = 2
  num_hmaster_master = 1

  config.ssh.insert_key = false
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true
  config.vm.box = "centos-7.1"
  config.vm.box_url = "https://dl.fedoraproject.org/pub/alt/purpleidea/vagrant/centos-7.1/centos-7.1.box"

  config.vm.provider "libvirt" do |libvirt, override|
    libvirt.cpus = 2
    libvirt.memory = 2048
    libvirt.driver = 'kvm'
  end

  num_hmaster_master.times do |n|
    config.vm.define "hmaster" do |hmaster_node|
      hmaster_node.vm.hostname = "hmaster.example.com"
      hmaster_node.vm.network :private_network, ip: "192.168.100.210"
      hmaster_node.vm.provider :libvirt do |libvirt|
			  libvirt.storage :file, :size => '10G'
		  end
    end
  end


  num_hslave_nodes.times do |n|
	  hslave_index = n+1
	  config.vm.define "hslave#{hslave_index}" do |hslave_node|
		  hslave_node.vm.hostname = "hslave#{hslave_index}.example.com"
		  hslave_node.vm.network :private_network, ip: "192.168.100.#{200 + n}"
      hslave_node.vm.network :forwarded_port, guest: 8443, host: 8443      
		  # Add extra disk, for gluster
		  hslave_node.vm.provider :libvirt do |libvirt|
			  libvirt.storage :file, :size => '10G'
		  end

		  # We only want to run ansible once
		  if hslave_index == num_hslave_nodes
			  hslave_node.vm.provision :ansible do |ansible|
          ansible.limit = 'all'
          ansible.sudo = true
          ansible.groups = {
            "hslaves" => ["hslave1", "hslave2"],
            "hmasters" => ["hmaster"],
            "hadoopmaster" => ["hmaster"],
            }
            ansible.extra_vars = {
              # This will tell ansible wich disk to use. This disk needs to be empty
				      gluster_install_disk: "/dev/vdb",
				      # Brick directory
				      gluster_brick_directory: "/data/brick1",
              gluster_mount_directory: "/mnt/gv0",
              gluster_mount_server: "hslave1.example.com",
              java_directory: "/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91-2.6.2.1.el7_1.x86_64/jre",
              }
              ansible.playbook = "ansible/playbook.yml"
        end
      end
    end
  end
end
