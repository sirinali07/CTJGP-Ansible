```
vi pckg.yaml
```
```
---
---
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
```
ansible-playbook pckg.yaml
```
