## LAB : Blocks with Ansible Playbook

### Create and edit blklab.yml in the same labs directory 
 Notice that the “web_package” variable is an invalid package. Due to the invalid package in a block, tasks under rescue will run
```
vi blklab.yml
```
```
---
- hosts: all
  become: yes
  user: ec2-user
  connection: ssh
  gather_facts: no
  tasks:
    - block:
        - name: Install {{ web_package }} package
          yum:
            name: "{{ web_package }}"
            state: latest
      rescue:
        - name: Install {{ db_package }} package
          yum:
            name: "{{ db_package }}"
            state: latest
      always:
        - name: Start {{ db_service }} service
          service:
            name: "{{ db_service }}"
            state: started
  vars:
    web_package: http
    db_package: mariadb-server
    db_service: mariadb
```
**save the file using** `ESCAPE + :wq!`

 Execute the playbook,
 Block tasks fail and that Rescue tasks are running due to the failure of block tasks. The Always tasks run independently
```
ansible-playbook blklab.yml
```
 Now fix the package name in the Playbook (web_package: httpd) and run the Playbook again
```
ansible-playbook blklab.yml
```

 Notice that the tasks under rescue are not running as block tasks ran successfully.
