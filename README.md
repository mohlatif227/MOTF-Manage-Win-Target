# MOTF-Manage-Win-Target
Manage WIndows Target Machine with Ansible

## Install Ansible on any Linux System
We're assuming that ansible is already installed on CentOS 7 Server. In case Ansible is not installed, please follow the official Ansible Website to install ansible on RHEL/CENTOS/FEDORA Server:

Please refer to this [page](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-rhel-centos-or-fedora)

## Setting up WinRM plugin on Ansible Control Node
WinRM is a management protocol used by Windows to remotely communicate with another server. It is a SOAP-based protocol that communicates over HTTP/HTTPS, and is included in all recent Windows operating systems. Since Windows Server 2012, WinRM has been enabled by default, but in most cases extra configuration is required to use WinRM with Ansible.

Ansible uses the `pywinrm` package to communicate with Windows servers over WinRM. It is not installed by default with the Ansible package, but can be installed by running the following:

`pip install "pywinrm>=0.3.0"`

