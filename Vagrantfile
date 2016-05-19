# -*- mode: ruby -*-
# vi: set ft=ruby :

#
#  MYSQL MULTI-NODE CLUSTER

#--------------------
#
# Items to cover
#
# box(es)
# host name(s)
# network
# shared folders
# chef omnibus install
# recipes to call
#
#--------------------

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

# Vagrant version
Vagrant.require_version '>= 1.5.0'


# CONFIGURATION - START

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  #  ------  GLOBAL SETTINGS BEGIN ------
  #  These will apply for all servers created by this file
  #

  # Box
  # No need for url as this box racattack/oracle65 is a known box
  config.vm.box = 'racattack/oracle65'
  #config.vm.box_url = 'https://storage.us2.oraclecloud.com/v1/istoilis-istoilis/vagrant/oel65-64.box'
  #config.ssh.password = 'vagrant'
  
  # Box override  if using vmware_fusion
  config.vm.provider "vmware_fusion" do |v, override|
    override.vm.box = 'bento/ubuntu-14.04'
  end

  # Shared folders
  #config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true
  #config.vm.synced_folder "../data", "/vagrant_data"

  #  ------  GLOBAL SETTINGS END ------
  #

  #  ------  SERVER DEFINITIONS BEGIN  -----------------
  #  Specify for either provider - will pickup whatever is relevant in the environment


   # Begin rac2n1

     # Begin rac2n1
    config.vm.define "rac2n1" do |rac2n1|

      # hostname
      rac2n1.vm.hostname = "rac2n1"

      # customization syntax for vmware 
      rac2n1.vm.provider "vmware_fusion" do |v|
          v.vmx["numvcpus"] = "1"
          v.vmx["memsize"] = "512"
      end
   
      # customization syntax for virtualbox
      rac2n1.vm.provider "virtualbox" do |v|
          #v.customize [ "modifyvm", :id, "--cpus", "1" ]
          #v.customize [ "modifyvm", :id, "--memory", "768" ]

          # ASM Disks for data/fra disk groups
          asm_disk_count=3
          file_name_pattern='C:\Users\username\VirtualBox VMs\VMDisks\rac2-asm'
          file_type='vmdk'

          #file_name= 'C:\Users\username\VirtualBox VMs\VMDisks\rac2-asm1.vmdk'

          (1..asm_disk_count).each do |disk|

            file_name="#{file_name_pattern}#{disk}.#{file_type}"
            port_number=disk+1

            unless File.exist?(file_name)
              v.customize ['createhd', '--variant', 'Fixed', '--filename', file_name, '--format', file_type, '--size', 2 * 1024]
              v.customize ['modifyhd', file_name , '--type', 'shareable']
            end

            v.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", port_number, "--device", 0, "--type", "hdd", '--medium', file_name]

          end

      end

      # Network
      # Public IP
      rac2n1.vm.network "private_network", ip: "192.168.0.111", virtualbox__intnet: true
      # VIP
      rac2n1.vm.network "private_network", ip: "192.168.0.11", virtualbox__intnet: true
      # Private Interconnect
      rac2n1.vm.network "private_network", ip: "10.10.10.11", virtualbox__intnet: true

      # Provisioning steps

      # Shell provisioner
      #rac2n1.vm.provision :shell, inline: 'echo rac2n1 shell provision echo'

      # Chef provisioner 
      #rac2n1.omnibus.chef_version = '12.0.4'

      #rac2n1.vm.provision :chef_solo do |chef|
      #  chef.run_list = [
      #  'recipe[rac2node::default]'
      #  ]
      #end

    end

   # End rac2n1

   # Begin rac2n2
    config.vm.define "rac2n2" do |rac2n2|
      rac2n2.vm.hostname = "rac2n2"
   
      rac2n2.vm.provider "vmware_fusion" do |v|
          v.vmx["numvcpus"] = "1"
          v.vmx["memsize"] = "512"
      end
   
      rac2n2.vm.provider "virtualbox" do |v|
          # Different way of setting cpu and memory compared to the other node above
          #v.cpus = "1"
          #v.memory = "768"

          # ASM Disks for data/fra disk groups
          asm_disk_count=3
          file_name_pattern='C:\Users\username\VirtualBox VMs\VMDisks\rac2-asm'
          file_type='vmdk'

          (1..asm_disk_count).each do |disk|

            file_name="#{file_name_pattern}#{disk}.#{file_type}"
            port_number=disk+1

            unless File.exist?(file_name)
              v.customize ['createhd', '--variant', 'Fixed', '--filename', file_name, '--format', file_type, '--size', 2 * 1024]
              v.customize ['modifyhd', file_name , '--type', 'shareable']
            end

            v.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", port_number, "--device", 0, "--type", "hdd", '--medium', file_name]

          end

          # EXAMPLE DISK-ADD BEGIN (KEEP THIS COMMENTED)
          #file_to_disk = 'C:\Users\username\VirtualBox VMs\VMDisks\rac2-asm1.vmdk'

          #unless File.exist?(file_to_disk)
            #v.customize ['createhd', '--variant', 'Fixed', '--filename', file_to_disk, '--size', 1 * 1024]
            #v.customize ['modifyhd', file_to_disk , '--type', 'shareable']
          #end
  
          #v.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", 2, "--device", 0, "--type", "hdd", '--medium', file_to_disk]
          # EXAMPLE END
      end

      # Network
      # Public IP
      rac2n2.vm.network "private_network", ip: "192.168.0.112", virtualbox__intnet: true
      # VIP
      rac2n2.vm.network "private_network", ip: "192.168.0.12", virtualbox__intnet: true
      # Private Interconnect
      rac2n2.vm.network "private_network", ip: "10.10.10.12", virtualbox__intnet: true

      # Provisioning steps

      # Shell provisioner
      #rac2n2.vm.provision :shell, inline: 'echo rac2n2 shell provision echo'

      # Chef provisioner 
      #rac2n2.omnibus.chef_version = '12.0.4' 

      #rac2n2.vm.provision :chef_solo do |chef|
      #  chef.run_list = [
      #  'recipe[rac2node::default]'
      #  ]
      #end     

    end
    # End rac2n2

    #  ------  SERVER DEFINITIONS END  -----------------

end  
# CONFIGURATION - END
