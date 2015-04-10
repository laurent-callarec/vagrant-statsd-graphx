# -*- mode: ruby -*-
# vi: set ft=ruby :

settings = {
 "network.ip" => "192.168.33.10"
}

Vagrant.configure("2") do |config|

  config.vm.box = "precise64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"

  #config.vm.network :forwarded_port, guest: 9200, host: 9200
  #config.vm.network :forwarded_port, guest: 12201, host: 12201, protocol: "udp"

  config.vm.network :private_network, ip: settings['network.ip']

  # forward ssh keys into vagrant
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "3048"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.omnibus.chef_version = :latest

  config.vm.provision :chef_solo do |chef|
    # List of recipes to run
    chef.cookbooks_path = ['chef/cookbooks', 'chef/local_cookbooks']
   
    chef.add_recipe "apt"
    chef.add_recipe "statsd"
    chef.add_recipe "graphite"
    chef.add_recipe "graphite::carbon"
    chef.add_recipe "graphite::whisper"    
    chef.add_recipe "graphite::dashboard"
    chef.add_recipe "grafana"    

    chef.json = {
      "statsd" => {"nodejs_bin" => "/usr/bin/node"},
      "graphite" => {
        "version" => "0.9.12"
      },
      "grafana" => {
        "webserver_port"   => "81",
        "webserver_listen" => ""
      }
    }

  end
end
