# Error Handling

**If we run an play book of 10 tasks. After compliting 2 tasks suppose there is an error on the 3rd task. If ansible catch any error in the task then ansible will not run the other pending tasks. If you want to ignore the error in any task then you can use `ignore_errors`**


## ignore_errors.

- Playbook.

```yaml
- name: learning ignore_errors
  hosts: webserver1
  become: yes
  tasks:

    - name: creating a file
      command: touch /home/ubuntu/tasks/Im_here.txt

    - name: installing docker
      apt:
        name: docker.io
        state: present
```

- there no such a path `/home/ubuntu/tasks` in my remote machine.

- So that my playbook gives me an error on the first task then second task will not execute.

- To ignore the error on the first task simply add this line `ignore_errors: true`

```yml
- name: learning ignore_errors
  hosts: webserver1
  become: yes
  tasks:

    - name: creating a file
      command: touch /home/ubuntu/tasks/Im_here.txt
      ignore_errors: true

    - name: installing docker
      apt:
        name: docker.io
        state: present
```

- Now ansible will ignore if any error accurs at the first task.