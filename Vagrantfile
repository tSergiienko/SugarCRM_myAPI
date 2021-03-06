# -*- mode: ruby -*-
# vi: set ft=ruby :

required_plugins = %w(
  vagrant-hostsupdater
  vagrant-triggers
)
plugins_to_install = required_plugins.select { |plugin| not Vagrant.has_plugin? plugin }
if not plugins_to_install.empty?
  puts "Installing plugins: #{plugins_to_install.join(' ')}"
  if system "vagrant plugin install #{plugins_to_install.join(' ')}"
    exec "vagrant #{ARGV.join(' ')}"
  else
    abort 'Installation of one or more plugins has failed. Aborting.'
  end
end

project = 'testPhp' # e.g. super-project
vm_ip = '192.168.100.108' # e.g. 192.168.x.x
web_root = '/vagrant/' # e.g. /vagrant/public

Vagrant.configure(2) do |config|
  config.vm.box = 'ubuntu/trusty64'
  config.vm.hostname = project
  config.vm.network 'private_network', ip: vm_ip

  config.vm.provider 'virtualbox' do |vb|
    vb.memory = 1024
  end

  config.vm.provision :shell, path: 'provision/system.sh'
  config.vm.provision :shell, path: 'provision/project.sh', args: [project, web_root]

  config.hostsupdater.aliases = ["#{project}.loc"]

  config.trigger.after :up do
    info '================================================'
    info "All set, visit http://#{project}.loc"
    info '================================================'
  end
end
