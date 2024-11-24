# Playbook

**Playbook is a set of tasks which is going to perform on the target host or hosts**

### 1. Now we are going to create one dummy file on the server.

- Create a inventory file for the server.

```yaml
webserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
```

- Now create playbook for this server.

```yaml
- name: this is our first play.
  hosts: webserver1
  tasks:
    - name: create a dummy file on webserver1
      command: touch /home/ubuntu/ansible_dummy.txt
```

- This is very simple to understand.

- Now run this command.

```bash
ansible-playbook playbook1.yaml -i inventory_servers.txt
```

- `ansible-playbook` is a command `playbook1.yaml` is my playbook name `-i` to give inventoryfile.

- Output of this command.

```bash
PLAY [this is our first play.] ***************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************
ok: [webserver1]

TASK [create a dummy file on webserver1] *****************************************************************************************************************************************************
changed: [webserver1]

PLAY RECAP ***********************************************************************************************************************************************************************************
webserver1                 : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

- Now verify your file on the server.

- Let me explain all the things in the output of this command.

- `PLAY [this is our first play.] ` Is the name of our playbook.

- `TASK [Gathering Facts] ` This is the first default task. It collects system information (called facts) about the target host(s), such as OS, IP addresses, memory, etc. This information can be used in subsequent tasks. `ok:` Indicates this task completed successfully without making changes. Facts are simply gathered, so no changes are made.

- `[webserver1]`: The target host on which the task was executed.

- `TASK [create a dummy file on webserver1]`: A task you defined in your playbook.

- `changed`: Indicates that the task made a change on the host.

- **`PLAY RECAP` :-**

- ok=2: Total tasks that ran successfully (including tasks that either made changes or only checked the system without changing anything). In this case:

Gathering Facts → ok

Creating the dummy file → changed

- changed=1: Number of tasks that actually made a change to the system (e.g., creating the file).

- unreachable=0: Number of hosts Ansible couldn’t connect to (e.g., due to network issues or incorrect SSH credentials). A non-zero value means Ansible couldn’t even start tasks on the host.

- failed=0: Number of tasks that failed during execution. A non-zero value means something went wrong during task execution.

- skipped=0: Tasks that were skipped (e.g., due to when conditions not being met).

- rescued=0: Tasks that were part of a rescue block, which is triggered if a failure occurs in a preceding task.

- ignored=0: Tasks that failed but were ignored due to the ignore_errors: yes option in the playbook.

## 2. How to Deal with and servers.

- Now we are going to run two commands on each two servers.

- inventory file.

```yaml
webserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver2 ansible_host=18.221.38.127 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem

[webservers]
webserver1
webserver2
```

- playbook.

```yaml
- name: this is our first play1.
  hosts: webserver1
  tasks:
    - name: create a dummy file on webserver1
      command: touch /home/ubuntu/ansible_web1.txt

- name: this is our first play2.
  hosts: webserver2
  tasks:
    - name: create a dummy file on webserver2
      command: touch /home/ubuntu/ansible_web2.txt

- name: this is our first play3.
  hosts: webservers
  tasks:
    - name: create a dummy folder on both webservers
      command: mkdir /home/ubuntu/webservers
```

- Now run this play book then verify all the things are runned ok.