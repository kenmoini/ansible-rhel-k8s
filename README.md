# ansible-rhel-k8s

Deploy a simple Kubernetes cluster on top of Red Hat Enterprise Linux

## Create some RHEL systems

First you need some RHEL systems - these can be bare metal or virtual.  There's a handy Ansible Playbook to create them on Libvirt/KVM.

```bash
ansible-playbook -e mode=create create-libvirt-vm.yml
```

Just wait for the VMs to shut down, then start them back up.

## Deploy Kubernetes

```bash
# Install needed collections/roles
ansible-galaxy collection install community.general
ansible-galaxy collection install ansible.posix
ansible-galaxy install git+https://github.com/geerlingguy/ansible-role-security.git
ansible-galaxy install geerlingguy.swap

#ansible-galaxy install geerlingguy.kubernetes
ansible-galaxy install git+https://github.com/kenmoini/ansible-role-kubernetes
rm -rf ~/.ansible/roles/geerlingguy.kubernetes
mv ~/.ansible/roles/ansible-role-kubernetes ~/.ansible/roles/geerlingguy.kubernetes

ansible-playbook -i .generated/gen_inventory deploy-k8s.yml
```