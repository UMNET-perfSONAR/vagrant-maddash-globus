# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  ###### Maddash Webui #####
  config.vm.define "server" do |server|
    config.vm.box = "centos/7"

    # Create a private network, which allows host-only access to the machine
    config.vm.network "private_network", ip: "192.1.1.12"

    config.vm.provider "virtualbox" do |vb|
      # Display the VirtualBox GUI when booting the machine
      #vb.gui = true
        # Customize the amount of memory on the VM:
      vb.memory = "1024"
    end

    config.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.inventory_path = "./inventory"
      ansible.groups = {
        "all:vars" => {
	        "ansible_become" => "True",
	        "ansible_become_user" => "root",
	        "ansible_become_method" => "sudo",
	        "perfsonar_os_update" => "False"
        },
        "ps-testpoints:vars" => {
	        "perfsonar_archive_auth_interfaces" => "{{ ansible_all_ipv4_addresses }}",
        },

      "ps-maddash" => [server],
      }
      ansible.limit = "all",
      ansible.playbook = "perfsonar.yml"
    end #vm.provision
  end #vm.define server

  ###### Archiver ######
  config.vm.define "esmond" do |esmond|
    config.vm.box = "centos/7"

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    config.vm.network "private_network", ip: "192.1.1.13"

    config.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end

    config.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.inventory_path = "./inventory"
      ansible.groups = {
        "all:vars" => {
	        "ansible_become" => "True",
	        "ansible_become_user" => "root",
	        "ansible_become_method" => "sudo",
	        "perfsonar_os_update" => "False"
        },
        "ps-archives" => ["esmond"],
        "ps-psconfig-publishers" => ["esmond"],

        "ps-testpoints:vars" => {
	        "perfsonar_archive_auth_interfaces" => "{{ ansible_all_ipv4_addresses }}",
          "perfsonar_archive_hosts" => "{{ groups['ps-archives'] }}"
        },
      }
      ansible.limit = "all",
      ansible.playbook = "perfsonar.yml"
    end #vm.provision
  end #vm.define esmond

  ###### Testpoint and gridftp server ######
  config.vm.define "centos7" do |centos7|
    config.vm.box = "centos/7"

    config.vm.network "private_network", ip: "192.1.1.14"

    config.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end

    config.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.inventory_path = "./inventory"
      ansible.groups = {
        "all:vars" => {
	        "ansible_become" => "True",
	        "ansible_become_user" => "root",
	        "ansible_become_method" => "sudo",
	        "perfsonar_os_update" => "False"
        },

        "ps-testpoints:vars" => {
	        "perfsonar_archive_auth_interfaces" => "{{ ansible_all_ipv4_addresses }}",
          "perfsonar_archive_hosts" => "{{ groups['ps-archives'] }}"
        },
        "ps-testpoints" => [centos7],
        "ps-toolkits" => [centos7],
      }
      ansible.limit = "all",
      ansible.playbook = "perfsonar.yml"
    end #vm.provision
  end #vm.define centos7
  
end