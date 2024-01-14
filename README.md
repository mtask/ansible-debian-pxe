## Ansible role | debian-pxe

This repository contains an Ansible to role to setup PXE boot server for Debian workstation installation. Role is tested with Debian 12.
  
Role's variables are set in defaults/main.yml and explained in comments there.
  
Role installs Dnsmasq to target hosts with TFTP and DHCP server configuration. DNS Server is not enabled in configuration.
  
Nginx server is configured on the same host to serve preseed configuration file. Role only configures one default preseed file and pxelinux menu entry. 
It doesn't support invidual pxelinux.cfg entries or preseed files. 
  
Prompts that are not automated by design:

* root user's password.
* "normal" user's password (username is set in variables).
* LUKS encryption password.
  
Used preseed file is created from template workstation_preseed.cfg.j2 which can be configured to server different needs.
  
Role contains a file named deploy.sh under the "files" directory. This is hosted on the nginx server and configured in preseed late commands to be executed at the end of the installation.
It contains some placeholder things but can be modified to do anything. Just remember that it is executed during the installation so you need to use `in-target` commands and `/target/` path accordingly.

### Usage

* Set variables correctly for your environment (check `defaults/main.yml`)
* Call role against PXE server host in a playbook. For example:

```yml
- hosts: pxe
  tasks:
  - name: Deploy PXE server
    import_role:
      name: debian-pxe
```
* Boot a machine with PXE and select "debian 12 automated" from the GRUB menu (first entry).
