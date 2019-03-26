unless Vagrant.has_plugin?('vagrant-libvirt')
  puts 'vagrant-libvirt plugin not found, installing'
  system 'vagrant plugin install vagrant-libvirt'
  exec "vagrant #{ARGV.join(' ')}"
end

unless Vagrant.has_plugin?('vagrant-mutate')
  puts 'vagrant-mutate plugin not found, installing'
  system 'vagrant plugin install vagrant-mutate'
  exec "vagrant #{ARGV.join(' ')}"
end

unless Vagrant.has_plugin?('vagrant-reload')
  puts 'vagrant-reload plugin not found, installing'
  system 'vagrant plugin install vagrant-reload'
  exec "vagrant #{ARGV.join(' ')}"
end

Vagrant.configure("2") do |config|
  config.vm.box = "archlinux/archlinux"
  config.vm.provider "virtualbox" do |vbox|
    vbox.memory = 4096
    vbox.cpus = 4
    lfs_disk = "/home/dkrm/dev/ft_linux/lfs.vdi"
    
    if not File.exists?(lfs_disk)
      vbox.customize ['createmedium',
                      '--filename', lfs_disk,
                      '--variant', 'Fixed',
                      '--size', 20 * 1024]
    end 
    
    if not File.exist?(".vagrant/machines/default/virtualbox/action_provision")
      vbox.customize ['storagectl', :id,
                      '--name', 'SATA Controller',
                      '--add', 'sata',
                      '--portcount', 2]
    end
    
    vbox.customize ['storageattach', :id,
                    '--storagectl', 'SATA Controller',
                    '--port', 1,
                    '--device', 0,
                    '--type', 'hdd',
                    '--medium', lfs_disk]
  end
  config.vm.provision "00-version-check", type: :shell, path: "00-version-check.sh"
  config.vm.provision "10-setup-disk",    type: :shell, path: "10-setup-disk.sh"
end
