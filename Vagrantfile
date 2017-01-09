# -*- mode: ruby -*-
# vi: set ft=ruby :

CONFIG = {
  vagrantApiVersion: '2',
  'image' => {
    name: 'arnemertz/Xubuntu16.04'
  },
  'system' => {
    hostname: 'hobbes-dev-1',
    ip: '192.168.192.10',
    memory: '2048',
    gui: true
  }
}

Vagrant.configure(CONFIG[:vagrantApiVersion]) do |config|
  config.vm.box       = CONFIG['image'][:name]
  config.vm.hostname  = CONFIG['system'][:hostname]
  config.vm.network :private_network, ip: CONFIG['system'][:ip]

  config.vm.provider 'virtualbox' do |v|
    # Documentation of modifyvm parameters
    # https://www.virtualbox.org/manual/ch08.html#vboxmanage-modifyvm

    v.gui    = CONFIG['system'][:gui]
    v.name   = CONFIG['system'][:hostname]

    v.memory = CONFIG['system'][:memory]

    v.cpus = 2
    v.customize ['modifyvm', :id, '--cpuexecutioncap', '50'] # Use not more than 50% host cpu

    # https://serverfault.com/questions/453185/vagrant-virtualbox-dns-10-0-2-3-not-working
    v.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    v.customize ['modifyvm', :id, '--natdnsproxy1', 'on']

    v.customize ['modifyvm', :id, '--ioapic', 'on']
    v.customize ['modifyvm', :id, '--hwvirtex', 'on']
    v.customize ['modifyvm', :id, '--rtcuseutc', 'on']
    
    v.customize ['modifyvm', :id, '--accelerate3d', 'on']
    v.customize ['modifyvm', :id, '--vram', '256']
    v.customize ['modifyvm', :id, '--monitorcount', '2']

    v.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
    v.customize ['modifyvm', :id, '--draganddrop', 'bidirectional']
  end

  config.vm.provision 'shell', path: './Vagrantfile_bootstrap.sh'

  config.vm.provision 'ansible_local' do |ansible|
    ansible.playbook = 'cm/site.yml'
  end
end
