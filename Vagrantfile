# This Vagrantfile can be used to develop Vagrant.

VAGRANTFILE_API_VERSION = "2"
 
#Vagrant version file
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #Vagrant base Box
  config.vm.box = "ubuntu/trusty64"
 
  #Specific provider  
["vmware_fusion", "vmware_workstation", "virtualbox"].each do |provider|
  config.vm.provider provider do |v, override|      
    #set Memory      
    v.memory = "1024"      
    #Set CPU
    v.cpus = 2
  end
end
  
#Set Private network by DHCP
  config.vm.network "private_network", type: "dhcp"
 
 #Specify Plugins cfg
  config.hostmanager.enabled = true
  config.hostmanager.ip_resolver = proc do |vm, resolving_vm|
    if vm.id
      `VBoxManage guestproperty get #{vm.id} "/VirtualBox/GuestInfo/Net/1/V4/IP"`.split()[1]
    end
  end
 
 #Define a Virtual machine A
  config.vm.define :server do |srv|
    srv.vm.hostname = "nagios-server"
    srv.vm.synced_folder "server/", "/usr/local/nagios/etc", create: true
    srv.vm.network "forwarded_port", guest: 80, host: 8080
    srv.vm.provision "shell", path: "server-provision"
  end
 
  #Define a Virtual machine B
  config.vm.define :client do |cl|
    cl.vm.hostname = "nagios-client"
    cl.vm.synced_folder "client/", "/usr/local/nagios/etc", create: true
    cl.vm.provision "shell", path: "client-provision"
  end
end
