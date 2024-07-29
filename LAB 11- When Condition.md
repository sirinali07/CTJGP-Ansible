```
vi pckg.yaml
```
Add the given content, by pressing "INSERT"
```yaml
- name: Installing list of packages
  hosts: all
  become: yes
  tasks:
  - name: install packages
    dnf:
      name: "{{ item }}"
      state: present
    loop:
      - tree
      - curl
      - wget
      - telnet
      - vsftpd
      - bind-utils
    when: ansible_facts['distribution'] == 'RedHat'

```
save the file using ESCAPE + :wq!

execute your Ansible playbook to install the specified packages:
```
ansible-playbook pckg.yaml
```
After running the playbook, check if the packages are installed on the managed nodes:

