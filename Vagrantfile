Vagrant.configure("2") do |config|
  
  #Number of data nodes to provision 
  numDataNodes = 1 

  #Number of client nodes to provision
  numClientNodes = 1

  #Master IP address
  ipAddrPrefix = "192.168.240.100"

  if Vagrant.has_plugin?("vagrant-vbguest") then 
    config.vbguest.auto_update = false
  end

  if Vagrant.has_plugin?("vagrant-cachier") then 
    config.cache.scope = :machine
  end

  config.vm.synced_folder ".","/vagrant_data"

  config.vm.define "master" do |web|
    web.vm.box = "ubuntu/trusty64"
    web.vm.hostname = "master"
    web.vm.network "private_network", ip: ipAddrPrefix
    web.vm.provider  "virtualbox" do |v|
      v.customize ["modifyvm",:id,"--memory","2048"]
      v.name = "Elastic MasterNode"
    end
    web.vm.provision "shell", :path => "master/bootstrap.sh"
    web.vm.network :forwarded_port, guest: 9200,host: 9200
  end

  1.upto(numDataNodes) do |num|
    dataNodeName = ("node" +num.to_s).to_sym
    config.vm.define dataNodeName do |datanode|
    datanode.vm.box = "ubuntu/trusty64"
    datanode.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm",:id,"--memory","2048"]
      v.name = "Elastic DataNode "+num.to_s
    end
    datanode.vm.network "private_network",ip: ipAddrPrefix + num.to_s
    datanode.vm.provision "shell", :path => "datanode/bootstrap.sh"
  end  
  end
end

