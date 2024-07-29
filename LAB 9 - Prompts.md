## LAB : Prompts with Ansible Playbooks

Create and edit promptlab.yml in the same labs directory 
```
vi promptlab.yml
```
```
---
- hosts: all
  become: yes
  user: ec2-user
  connection: ssh
  vars_prompt:
    - name: pkginstall
      prompt: Which package do you want to install?
      default: telnet
      private: no
  tasks:
    - name: Install the package specified
      yum: pkg={{ pkginstall }} state=latest
```

**save the file using** `ESCAPE + :wq!`

Execute the playbook & you will be prompt for enter the package name which you want to install
If no package is mentioned, telnet is installed by default
```
ansible-playbook promptlab.yml
```

Verify if the specified package httpd is installed. SSH into one of the machines and verify using the command 
```
ssh ec2-user@< managed_node_private_ip >
```
```
rpm -qa | grep httpd
```
```
exit
````
Once you back to "control-Node", run the following command to verify package from Control-Node
```
ansible all -m "command" -a "rpm -qa | grep httpd"
```
