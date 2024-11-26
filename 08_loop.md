# Loop

**We use loops in Ansible to repeat tasks for multiple items without writing the same task multiple times. It makes playbooks shorter, easier to read, and reusable.**

- A simple loop example to install many packages.

- Create a file to store name of all the packages.

```yaml
packages:
  - sl
  - cowsay
  - figlet
```

- These are packages i want to install on remote server.

```yaml
- name: learning loop of ansible
  hosts: webserver1
  become: yes
  vars_files:
    - packages.yaml
  tasks:
    - name: installing all the needed packages
      apt:
        name: "{{ packages }}"
        state: present
```

- If you face this error.

```bash
fatal: [webserver1]: FAILED! => {"changed": false, "msg": "This module does not currently support using glob patterns, found '[' in service name: ['package1', 'package2']"}
```

- Then you can yous items.

```yaml
- name: learning loop of ansible
  hosts: webserver1
  become: yes
  vars_files:
    - packages.yaml
  tasks:
    - name: installing all the needed packages
      apt:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"
```

- Playbook loop for services

```yaml
- name: learning loop
  hosts: webserver1
  become: yes
  vars_files:
    - services.yml
  tasks:
    - name: stoping some services
      service:
        name: "{{ item }}"
        state: stopped
      loop: "{{ services }}"
```