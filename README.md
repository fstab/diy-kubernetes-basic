Basic Kubernetes Setup with Terraform and Ansible
=================================================

Create a basic Kubernetes cluster on Linux root servers in the [Hetzner cloud](https://hetzner.cloud).

* [Terraform](https://www.terraform.io) script to set up the hosts.
* [Ansible](https://www.ansible.com) playbook to install Kubernetes using kubeadm.

This is a stripped down version of [github.com/fstab/diy-kubernetes](https://github.com/fstab/diy-kubernetes). It does not include the persistent volume, the load balancer, and Kubernetes monitoring.

**This repository is not maintained.** It contains a snapshot used in March 2019, but it will not be updated for future Kubernetes versions.

What You Get
------------

* Three hosts: `kube-master`, `kube-node-1`, `kube-node-2`.
* [Tinc VPN](https://www.tinc-vpn.org/) between the hosts. Kubernetes will use the VPN.
* `kubectl` ready to use on `kube-master`.

What You Need
-------------

1. Hetzner API token from [Hetzner Cloud Console](https://console.hetzner.cloud/) -> Access -> Tokens.
2. SSH key uploaded to [Hetzner Cloud Console](https://console.hetzner.cloud/) -> Access -> SSH Keys.
3. SSH key available locally (run `ssh-add <key>`), so that you can log into Hetzner machines without password.

How to Run
----------

1. Install [Terraform](https://www.terraform.io/) and [Ansible](https://www.ansible.com/).
2. Run `terraform init`. This should create a directory structure in `./.terraform/` and download the [provider.hcloud](https://github.com/terraform-providers/terraform-provider-hcloud) and the [provider.null](https://github.com/terraform-providers/terraform-provider-null).
3. Create a file `./terraform.tfvars` with your Hetzner API token and the name of the SSH key as follows:

```properties
hcloud_token="..."
ssh_key_name="..."
```

4. Run `terraform apply`, confirm with `yes`. This should start the servers, and generate an Ansible inventory config file `./inventory`.
5. `export ANSIBLE_HOST_KEY_CHECKING=False` to disable strict host key checking for Ansible (don't check `~/.ssh/known_hosts`).
6. Run `ansible-playbook -i ./inventory ./kubernetes.yml`.
