# Asynchronous Actions and Polling

**Sometimes, you need to run tasks that take a long time to finish, like installing software or running scripts. If you wait for these tasks to finish, your playbook can get stuck for a long time. Instead, you can run these tasks asynchronously in the background.**

### Let's do one example.

- Playbook.

```yml
- name: Example of Async Task
  hosts: localhost
  tasks:
    - name: Run a long task in the background
      apt:
        name: "{{ package }}"
        state: present
      async: 60
      poll: 10
```

- `async` will wait for this task only 60 seconds.

- `poll` will check the the task is completed or not after and after which seconds we gave. In this playbook poll will check the task is completed or failed after 10 seconds gap.

- If the task is not completed in the given time. Then ansible will add this task in the background and move to the next tasks.

- **Note :-** after adding the task in backgrounf ansible will not know the status of the task like task is `completed` or `failed`