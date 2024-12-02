# Strategies

**In Ansible, strategies decide how tasks run on multiple hosts:**

**1. `Linear (default):` Runs tasks on all hosts one by one in order.**

**2. `Free:` Runs tasks on hosts as they become ready, without waiting for others.**


## Let's do some examples of these strategies.

### 1. Linear

- `linear` is the default startegy used my ansible playbooks.

```yml
- name: learning strategies
  hosts: webserver1 sqlserver1
  become: yes
  tasks:

    - name: installing docker.io
      apt:
        name: docker.io
        state: present

    - name: installing docker-compose
      apt:
        name: docker-compose
        state: present
```

- When we run this playbook. **`Ansible`** will run first task on the both servers `webserver1` and `sqlserver1`. Until the first task is not completed on both remote servers ansible will not execute the second task.

### 2. Free

- `free`

```yaml
- name: learning strategies
  hosts: webserver1 sqlserver1
  strategy: free
  become: yes
  tasks:

    - name: installing docker.io
      apt:
        name: docker.io
        state: present

    - name: installing docker-compose
      apt:
        name: docker-compose
        state: present
```

- `Free:` Runs tasks on hosts as they become ready, without waiting for others.