Vagrant.require_version ">= 1.7.0"

$os_image = (ENV['OS_IMAGE'] || "ubuntu16").to_sym

def set_vbox(vb, config)
  vb.gui = false
  vb.memory = 1024
  vb.cpus = 1

  case $os_image
  when :centos7
    config.vm.box = "bento/centos-7.2"
  when :ubuntu16
    config.vm.box = "bento/ubuntu-16.04"
  end
end

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox"

  private_count = 10
  (1..3).each do |mid|
    name = 'kd'
    config.vm.define "#{name}#{mid}" do |n|
      n.vm.hostname = "#{name}#{mid}"
      ip_addr = "172.16.35.#{private_count}"
      n.vm.network :private_network, ip: "#{ip_addr}",  auto_config: true

      n.vm.provider :virtualbox do |vb, override|
        vb.name = "#{n.vm.hostname}"
        set_vbox(vb, override)
      end
      private_count += 1
      
      if mid == 3
        n.vm.provision "cluster", type: "ansible" do |ansible|
          ansible.playbook = "site.yml"
          ansible.inventory_path = "inventory"
          ansible.limit = "all"
          ansible.host_key_checking = false
        end
      end
    end
  end
end
