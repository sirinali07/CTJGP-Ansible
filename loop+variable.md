


```yaml
---
- name: This is the first play
  hosts: all
  become: yes
  vars_files:
  - variable.yaml
  tasks:
   - name: creating users
     user:
       name: "{{ item.name }}"
       uid: "{{ item.uid }}"
       state: present
     loop: "{{ user_list }}"

```

```yaml
user_list:
      - { name: 'u1', uid: '8010' }
      - { name: 'u2', uid: '8020' }
      - { name: 'u3', uid: '8030' }
      - { name: 'u4', uid: '8040' }
```
