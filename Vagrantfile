# -*- mode: ruby -*-
# vi: set ft=ruby :

CONFIG = {
  vagrantApiVersion: '2',
  'image' => {
    name: 'xenial-server-cloudimg-amd64-vagrant',
    url: 'https://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-vagrant.box'
  },
  'system' => {
    hostname: 'hobbes-dev-1',
    ip: '192.168.192.1',
    memory: '2048',
    gui: true
  }
}

Vagrant.configure(CONFIG[:vagrantApiVersion]) do |config|
  config.vm.box       = CONFIG['image'][:name]
  config.vm.box_url   = CONFIG['image'][:url]
  config.vm.hostname  = CONFIG['system'][:hostname]
  config.vm.network :private_network, ip: CONFIG['system'][:ip]

  config.vm.provider 'virtualbox' do |v|
    v.gui    = CONFIG['system'][:gui]
    v.name   = CONFIG['system'][:hostname]
    v.memory = CONFIG['system'][:memory]

    # https://serverfault.com/questions/453185/vagrant-virtualbox-dns-10-0-2-3-not-working
    v.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    v.customize ['modifyvm', :id, '--natdnsproxy1', 'on']

    v.customize ['modifyvm', :id, '--ioapic', 'on']

    v.customize ['modifyvm', :id, '--cpuexecutioncap', '50'] # Use not more than 50% host cpu
    #v.customize ['modifyvm', :id, '--monitorcount', '2'] # Use two monitors from host
  end

  config.vm.provision 'shell', path: './Vagrantfile_bootstrap.sh'

  config.vm.provision 'ansible_local' do |ansible|
    ansible.playbook = 'cm/site.yml'
  end
end
