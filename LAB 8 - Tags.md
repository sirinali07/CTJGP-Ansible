## LAB : Tags with Ansible Playbooks

Create and edit tagslabs.yml in the same labs directory 

```
vi tagslabs.yml
```
```
---
- hosts: all
  become: yes
  user: ec2-user
  connection: ssh
  gather_facts: no
  tasks:
    - name: Install telnet
      yum: pkg=telnet state=latest
      tags:
        - packages
    - name: Verifying telnet installation
      raw: yum list installed | grep telnet > /home/ec2-user/pkg.log
      tags:
        - logging
```

**save the file using** `ESCAPE + :wq!`

Execute the playbook using tags. Notice that only the tasks associated with the mentioned tags are running
```
ansible-playbook --tags "logging" tagslabs.yml
```
Execute the playbook using tags. Notice that  the tasks associated with the mentioned tags is skipping and remaining task are running
```
ansible-playbook --skip-tags "logging" tagslabs.yml
```
