# -*- mode: ruby -*-
# vi: set ft=ruby :

vms = {
'master' => {'memory' => '2048', 'cpus' => '2', 'ip' => '10', 'box' => 'ubuntu/focal64'},
'worker-one' => {'memory' => '2048', 'cpus' => '2', 'ip' => '11', 'box' => 'ubuntu/focal64'},
'worker-two' => {'memory' => '2048', 'cpus' => '2', 'ip' => '12', 'box' => 'ubuntu/focal64'},
'worker-three' => {'memory' => '2048', 'cpus' => '2', 'ip' => '13', 'box' => 'ubuntu/focal64'},
}

Vagrant.require_version ">= 1.8.0"

Vagrant.configure('2') do |config|
  vms.each do |name, conf|
     config.vm.define "#{name}" do |my|
       my.vm.box = conf['box']
       my.vm.hostname = "#{name}"
       #my.disksize.size = '30GB'
       my.vm.network 'private_network', ip: "172.16.33.#{conf['ip']}"
       #my.vm.network 'public_network', ip: "192.168.3.#{conf['ip']}", bridge: "wlp0s20f3"
       #my.vm.provision 'shell', path: "script.sh"
       my.vm.provision "ansible" do |ansible|
          ansible.playbook = "playbook.yml"
          ansible.host_vars = {
            "master" => {"single_controlplane" => true},
            "worker-one" => {"workers" => true},
            "worker-two" => {"workers" => true},
            "worker-three" => {"workers" => true},
          }
          ansible.extra_vars = {
            ansible_user: "vagrant"
          }
      end
       my.vm.provider 'virtualbox' do |vb|
          vb.memory = conf['memory']
          vb.cpus = conf['cpus']
       end
     end
  end
end

