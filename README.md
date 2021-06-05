# Food-game K8s dashboard
This repository contains the files needed to deploy an Openstack VM (based on Ubuntu) that contains all the necessary files and dependencies to run the dashboard application.

## Requirements
To run the scripts in this folder you need to have an Openstack installation available, with an empty project.
You then need to get the [Ubuntu Bionic cloud image](https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img). Its name must be `ubuntu-server-cloudimg-amd64.img`.
The file `ssh_key.pub` must contain the public key that will be installed in the NVM, both for the user ubuntu and the user eval.
In the `.env.backend` you need to put the configuration that will be used by the dashboard backend to connect to the K8s cluster. Please note that the fields in this file must be filled **before** running the Ansible playbooks (server-up.yml).
All the Ansible playbooks must then be filled with the credentials to login to Openstack. The provided user must have read and write permissions for the project.

## Running the scripts
After creating the empty Openstack project and filling in all the necessary configuration, you can run the first playbook with the command:

    ansible-playbook infrastructure-up.yml
This will upload the Ubuntu image to Openstack and create all the necessary things for the NVM to run (like network, subnet, SSH keypair, etc...)
After that playbook has finished running, you can create the NVM with:

    ansible-playbook server-up.yml
This command will create the NVM, assign the floating IP (which must be available), install all the dependencies in the NVM and download the dashboard code from Github.

## Deleting the NVM
To delete the NVM simply run:

	ansible-playbook server-down.yml
To delete the infrastructure previously created:

	ansible-playbook infrastructure-down.yml

