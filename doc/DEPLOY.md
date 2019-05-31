Deployment to DoC Cloud
==========

## VM Creation

Login to cloudstack with the doc domain
Create a new instance with the following details:
* Select a zone: doc-46
* Select ISO or template: Template
* Template: Ubuntu Ubuntu v16.04.3 (Non CSG - 2017/11/02)
* Compute offering	CPU: CPU: 2 cores 2GHz & RAM: 2Gb
* Disk: Local Storage: 20Gb

## VM Setup

* SSH into the VM from the Cloudstack console
* Create a user named Ubuntu
```bash
adduser --ingroup sudo ubuntu
```
* Login ubuntu and add your SSH key to ease the login process for ansible
```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
vim ~/.ssh/authorized_keys
```
* Run `sudo visudo` and at the end of the file add:
```ubuntu ALL=(ALL) NOPASSWD: ALL```
##### Note: This step is dangerous we should eventually update the process

## Deployment
* Open the file provisioning/hosts
* Add the VM hostname to all the roles you wish to be deployed on the VM in this format
```cloud-vm-XX-YY.doc.ic.ac.uk:22 ansible_ssh_user=ubuntu```
* Run
```bash
ansible-galaxy install -r ./requirements.yml -f
ansible-playbook -i provisioning/hosts --vault-password-file=~/.klee_vault_password provisioning/production.yml -v
```
