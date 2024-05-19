# kubernetes-ansible

## Dependencies

- ansible == 2.16.5

## SO tested and support

- Ubuntu 20

## Variables

| Name | Description | Default | Mandatory 
|------|-----------|---------|------------|
| master | Enabled master-controlplane  | false | yes
| workers | Enabled workers  | false | yes
| version_k8s | version k8s | v1.26 | no
| network_plugin | network plugin addon (weave/cilium/flannel)  | weave | no
| ingress_class | controler-ingress-class (nginx)  | nginx | no
| controller_openebs | controler-volumes (openebs)  | true | no
| container_runtime | container runtime (docker/containerd) | containerd | no
| vagrant | running use vagrant | true | no

## Example playbook

```yaml
---
- name: Kubernetes Installation master
  hosts: master
  vars:
    network_plugin: "weave"
    controller_openebs: true
    ingress_class: "nginx"
    master: true
    container_runtime: "docker"
  become: yes
  roles:
  - kubernetes  

- name: Kubernetes Installation workers
  hosts: workers
  vars:
    workers: true
    container_runtime: "docker"
  become: yes
  roles:
  - kubernetes  
```

**In the groups_vars directory you can set the variables for each host**

 - all will be applied to all, and the others separated by their respective groups

## Example file inventory

```
[master]
172.16.33.12 ansible_host=172.16.33.12 ansible_ssh_private_key_file=.vagrant/machines/master/virtualbox/private_key 
[workers]
172.16.33.13 ansible_host=172.16.33.13 ansible_ssh_private_key_file=.vagrant/machines/worker-1/virtualbox/private_key 
172.16.33.14 ansible_host=172.16.33.14 ansible_ssh_private_key_file=.vagrant/machines/worker-2/virtualbox/private_key 

[all:vars]
ansible_user=vagrant
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
#ansible_password="Mypassword"

```

***You can use vagrant***

### For instalation:

 [vagrant](https://www.vagrantup.com/downloads)

 [virtualbox](https://www.virtualbox.org/wiki/Downloads)

- Provision the VM

```bash
vagrant up 
```
- Access via ssh

```bash
vagrant ssh
```
- Shutdown the VM

```bash
vagrant halt
``` 

kubeconfig is saved in the current directory

```bash
export KUBECONFIG=./kubeconfig.yaml
```

### For execute

```bash
ansible-playbook -i inventory playbook.yml
```

ansible all -i inventory -m setup -a "filter=*ipv4*" -u vagrant

## License

GPLv3
