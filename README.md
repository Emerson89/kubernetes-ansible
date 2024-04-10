# kubernetes-ansible

## Dependencies

- ansible == 2.16.5

## SO tested and support

- Ubuntu 20

## Variables

| Name | Description | Default | Mandatory 
|------|-----------|---------|------------|
| master_ip | ip use master-controlplane | 127.0.0.1 | yes
| master | Enabled master-controlplane  | false | yes
| workers | Enabled workers  | false | yes
| version_k8s | version k8s | v1.26 | no
| network_plugin | network plugin addon (weave-cilium-flannel)  | weave | no
| ingress_class | controler-ingress-class (nginx)  | nginx | no
| controller_openebs | controler-volumes (openebs)  | openebs | no
| container_runtime | container runtime (docker-containerd) | containerd | no

## Example playbook

```yaml
---
- name: Kubernetes Installation master
  hosts: master
  vars:
    master_ip: 172.16.33.12
    network_plugin: "weave"
    controller_openebs: true
    ingress_class: "nginx"
    master: true
    container_runtime_docker: true
  become: yes
  roles:
  - kubernetes  

- name: Kubernetes Installation workers
  hosts: workers
  vars:
    workers: true
    container_runtime_docker: true
  become: yes
  roles:
  - kubernetes  
```

**In the groups_vars directory you can set the variables for each host**

 - all will be applied to all, and the others separated by their respective groups

## Example file inventory

```
[master]
172.16.33.15 ansible_ssh_private_key_file=.vagrant/machines/worker-1/virtualbox/private_key ansible_user=vagrant
[workers]
172.16.33.12 ansible_ssh_private_key_file=.vagrant/machines/worker-1/virtualbox/private_key ansible_user=vagrant
172.16.33.13 ansible_ssh_private_key_file=.vagrant/machines/worker-2/virtualbox/private_key ansible_user=vagrant
172.16.33.16 ansible_ssh_private_key_file=.vagrant/machines/worker-3/virtualbox/private_key ansible_user=vagrant

[all:vars]
ansible_user=vagrant
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
ansible-playbook -i hosts playbook.yaml
```

## License

GPLv3
 