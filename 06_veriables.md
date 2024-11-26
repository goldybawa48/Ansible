# Variables

**variables are used to make playbooks dynamic and reusable**

### 1. Direct variable in playbook

- playbook.

```yaml
- name: learning vars
  hosts: webserver1
  become: yes
  vars:
    service: nginx
  tasks:
    - name: installing nginx
      apt:
        name: "{{ service  }}"
        state: present

    - name: start the service nginx
      service: 
        name: "{{ service  }}"
        state: started
```

- This is a playbook to run nginx on the remote server.

### 2. Give variable file in playbook

- create variablefile `var.yml`.

```yaml
service: nginx
app: nginx
```

- Now create playbook.

```yaml
- name: learning vars
  hosts: webserver1
  become: yes
  vars_files:
    - var.yml
  tasks:
    - name: installing nginx
      apt:
        name: "{{ app }}"
        state: present

    - name: start the nginx service
      service: 
        name: "{{ service }}"
        state: started
```
