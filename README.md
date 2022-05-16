# MOTF: Managing Windows Host with Ansible
Manage Windows hosts with Ansible

## Prerequisites
Please be sure that you have following prerequisities to be configured first in order to manage Windows hosts using Ansible.

### Install Ansible Control Host on Linux System
* Please note that Windows is not supported as a control node, we're assuming that ansible is already installed on Linux host(Preferably CentOS 7 Server). In case Ansible is not installed, please follow the official Ansible page to install ansible on RHEL/CENTOS/FEDORA Server:

* Please refer to this [page](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-rhel-centos-or-fedora) for Ansible installation.
* Please make sure that python(python2.7 and python3.5 and higher is supported) is installed on Ansible Control host.
* The [pip package installed](https://pip.pypa.io/en/stable/installing/) on the Ansible control host.

## Setting up WinRM(Windows Remote Management) plugin on Ansible Control Host
WinRM is a management protocol used by Windows to remotely communicate with another server. It is a SOAP-based protocol that communicates over HTTP/HTTPS, and is included in all recent Windows operating systems. Ansible can easity manages your Windows notes too just like Linux/Unix nodes.

* Ansible uses the `pywinrm` package to communicate with Windows servers over WinRM. It is not installed by default with the Ansible package, but can be installed by running the following:

`pip install "pywinrm>=0.3.0"`



## Windows Managed Host Configuration

With most versions of Windows, WinRM ships in the box but isn’t turned on by default. There’s a [Configure Remoting for Ansible](https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1) script you can run on the remote Windows machine (in a PowerShell console as an Admin) to turn on WinRM.

## Configure ansible Inventory File Correctly
* In order to connect to your Windows hosts properly, you need to make sure that you put in `ansible_connection=winrm` in the [host vars](http://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW#host-variables) section of your inventory file so that Ansible Engine doesn’t just keep trying to connect to your Windows host via SSH.
* Also, the WinRM connection plugin defaults to communicating via https, but it supports different modes like message-encrypted http. Since the “Configure Remoting for Ansible” script we ran earlier set things up with the self-signed cert, we need to tell Python, “Don’t try to validate this certificate because it’s not going to be from a valid CA.” So in order to prevent an error, one more thing you need to put into the host vars section  is: `ansible_winrm_server_cert_validation=ignore` in your Inventory File.

Here is the example of Inventory file for Windows Host

```
[win]
AppServer01 ansible_host=<IP>

[win:vars]
ansible_user = WindowsUsername
ansible_password = Password
ansible_connection = winrm
ansible_winrm_transport = basic
ansible_winrm_server_cert_validation = ignore

```

## Test Communication
Now we have done all the configuration between Ansible Control Host and Windows Managed Host. Let's check to see if everything is working or not. In order to do so, go to your Ansible Control Host and run below command:

`ansible <host_group_name_in_inventory_file> -i <inventory_file_name> -m win_ping`

As per our Inventory file, the command would look like as below:

`ansible win -i inventory -m win_ping`

If everything is configured correctly, we should get the ping response as SUCCESS.

## Running Ansible Playbook
In order to run the ansible playbook, go to your Ansible Control Host and run the `ansible-playboook` command as below:

`ansible-playbook -i <inventory_file_name> <playbook_file_name>`

to run `install_chocolaty.yaml` ansible playbook, run the below command:

`ansible-playbook -i inventory install_chocolaty.yaml`


## Pending Task
Out of 4 tasks which was assigned to me to complete, I was not able to figure it out how to setup/configure ssh port configuration on Windows Managed host using Ansible playbook. I've always worked on Linux enviroment and I've never got opportunity to manage Windows host using Ansible, hence I couldn't find solution for that partcular task.

