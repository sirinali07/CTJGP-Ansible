## Lab : Implementing Jinja2 Templates

Create a new directory for templates 
```
mkdir templates
```
```
cd templates
```
In that directory create a template file, let’s say new-motd.j2
```
vi new-motd.j2
```
Put the below content into the newly created template file
```
System Information:
Operating System: {{ ansible_os_family | default('N/A') }}
Architecture: {{ ansible_architecture | default('N/A') }}
Distribution Version: {{ ansible_distribution | default('N/A') }}
```

Now create the playbook for implementing this jinja2 template that we created
```
vi implement-jinja2.yml
```
Put the below content in the playbook:
```
---
- name: playbook
  hosts: all
  gather_facts: true
  become: yes
  tasks:
    - name: Render template
      template:
        src: new-motd.j2
        dest: /etc/motd
        owner: ec2-user
        group: ec2-user
        mode: 0644
```
Now execute the playbook 
```
ansible-playbook implement-jinja2.yml -b 
```
 
 Now we will check what our template is exactly doing, where it is putting the values.
For that, do the following:
```
ansible all -m shell -a "cat /etc/motd"
```
OR, run the the following command to check "welcome Message" which trigered from "/etc/motd"
```
ssh ec2-user@<Managed-Node-IP>
```
```
exit
```
