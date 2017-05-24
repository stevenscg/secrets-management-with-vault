# Secrets Management with Vault

This repository provides a Vagrant environment with HashiCorp Vault, Consul, Nomad
and supporting tools installed that can be used to demonstrate many aspects of a
typical "Hashistack".


### Prerequisites
* Vagrant
* Virtualbox
* Ansible
* git


### Install Ansible and Virtualbox

If Vagrant and Virtualbox do not already exist on the host machine, install them now.

Ansible must be installed on the host machine to provision the VM and also to perform
each of the demonstration steps post-provisioning.

For MacOS:
```
sudo pip install ansible
```

Other platforms, see the [installation documentation](http://docs.ansible.com/ansible/intro_installation.html).


### Launch and provision the VM

From the host machine:
```
vagrant up
```

This installs a CentOS 7 base image plus the following:
* nginx
* docker
* consul
* dnsmasq
* vault
* nomad

See the provisioning playbook (`provision/playbook.yml`) for the roles and configuration
items used to bootstrap the VM.

The Vagrantfile instructs the required ansible roles from `requirements.yml` to be
downloaded prior to the start of provisioning.


### Stopping and Restarting the VM

When the VM is stopped and subsequently restarted, vault may remain sealed and processes that depend on vault (like nomad) may be unable to start.

To unseal using the built-in `dev-auth` vault helper, execute the following from within the VM:
```
sudo sh /opt/vault/helper/dev-auth.sh
```

To start nomad using systemd:
```
sudo service nomad start
```


### Using Vault

A vault token is required to access most vault functionality.

During vault initialization, the vault root token was captured by
the provisioning process to a well-known location on tmpfs:
`/var/run/vault/instance_token`.

Since tmpfs is cleared at boot time, if the instance_token file
is not available, the `dev-auth` vault helper should be executed
from within the VM:
```
sudo sh /opt/vault/helper/dev-auth.sh
```

To use the instance token, execute the following from within the VM:
```
export VAULT_TOKEN=$(cat /var/run/vault/instance_token)
```
