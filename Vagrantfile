
require "yaml"
configuration = YAML.load_file "configuration.yaml"



ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'

# Variables
VAGRANT_API_VERSION = configuration['vagrant_api_version']
host_os = configuration['host_os']
default_user = configuration['default_user']
servers = configuration['servers']
shell_commands = configuration['shell_commands']




Vagrant.configure(VAGRANT_API_VERSION) do |config|
  servers.each do|type, machines|
    machines.each do |name, specs|
      puts "SERVER: #{name}"
      config.vm.define name do |machine|
        puts "CONF: #{name} #{host_os} #{specs}"
        machine.vm.box = host_os
        machine.vm.hostname = name
        machine.vm.network "private_network", ip: specs['ip']
        machine.vm.provider "libvirt" do |virt|
          virt.memory = specs['memory']
          virt.cpus = specs['cpus']
          if specs['root_disk_size']
            virt.machine_virtual_size = specs['root_disk_size']
          end
        
        end

        machine.vm.provision "shell", inline: shell_commands
      end
    end
  end
    
end
