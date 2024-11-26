# File sepration

**We use file separation in Ansible to keep the playbooks, variables, and tasks organized. This makes it easier to manage large projects, reuse code, and update specific parts without affecting others.**

- example of a playbook.

```yaml
- name: learning file sepration
  hosts: webserver1
  vars_files:
    - variables.yml
  tasks:
    - include_tasks: tasks.yml
```

- To use this playbook i created a `variables.yml` file to store variables. Which looks like this.

```yaml
var1: var1
var2: var2
var3: var3
var4: var4
var5: var5
var6: var6
var7: var7
var8: var8
```

- Then i created a file for the task `tasks.yml`

```yaml
- name: task1
  command: touch ~/task/{{ var1 }}
    
- name: task2
  command: touch ~/task/{{ var2 }}

- name: task3
  command: touch ~/task/{{ var3 }}
    
- name: task4
  command: touch ~/task/{{ var4 }}

- name: task5
  command: touch ~/task/{{ var5 }}
    
- name: task6
  command: touch ~/task/{{ var6 }}

- name: task7
  command: touch ~/task/{{ var7 }}
    
- name: task8
  command: touch ~/task/{{ var8 }}
```

- After running this playbook the output is looks like.

```bash
ubuntu@ip-172-31-19-199:~/task$ ls
var1  var2  var3  var4  var5  var6  var7  var8
```

- We can porvide more then one or two `vars_files` and `include_tasks` files.