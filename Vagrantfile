Vagrant.require_version ">= 1.7.0"

Vagrant.configure(2) do |config|

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = "1"
  end

  config.vm.box = "ubuntu/xenial64"
  config.vm.define "worker1" do |w1|
    #w1.vm.network "forwarded_port", guest: 80, host: 8081
    w1.vm.network :private_network, ip: "172.16.0.101"
  end
  config.vm.define "worker2" do |w2|
    #w2.vm.network "forwarded_port", guest: 80, host: 8082
    w2.vm.network :private_network, ip: "172.16.0.102"
  end
  config.vm.define "loadbalancer1" do |l1|
    l1.vm.network "forwarded_port", guest: 80, host: 8080
    l1.vm.network :private_network, ip: "172.16.0.201"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "provisioning/workers.yml"
    ansible.groups = {
      "workers" => ["worker[1:2]"]
    }
  end

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "provisioning/loadbalancers.yml"
    ansible.groups = {
      "loadbalancers" => ["loadbalancer1"]
    }
  end

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "provisioning/sec-updates.yml"
  end
end
