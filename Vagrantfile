# -*- mode: ruby -*-
# vi: set ft=ruby :

vms = {
'master' => {'memory' => '3048', 'cpus' => '4', 'ip' => '12', 'box' => 'ubuntu/focal64'},
'worker-1' => {'memory' => '1048', 'cpus' => '2', 'ip' => '13', 'box' => 'ubuntu/focal64'},
'worker-2' => {'memory' => '1048', 'cpus' => '2', 'ip' => '14', 'box' => 'ubuntu/focal64'},
}

Vagrant.require_version ">= 1.8.0"

Vagrant.configure('2') do |config|
  vms.each do |name, conf|
     config.vm.define "#{name}" do |my|
       my.vm.box = conf['box']
       my.vm.hostname = "#{name}.example.com"
       #my.disksize.size = '30GB'
       my.vm.network 'private_network', ip: "172.16.33.#{conf['ip']}"
       #my.vm.network 'public_network', ip: "192.168.3.#{conf['ip']}", bridge: "wlp1s0"
       my.vm.provision "ansible" do |ansible|
          ansible.playbook = "playbook.yml"
          ansible.groups = {
            "master" => ["master"],
            "workers" => ["worker-1","worker-2"],
          }
          ansible.extra_vars = {
            vagrant: true
          }

       end
       my.vm.provider 'virtualbox' do |vb|
          vb.memory = conf['memory']
          vb.cpus = conf['cpus']
       end
     end
  end
end

