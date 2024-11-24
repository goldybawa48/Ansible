# Inventory File

**The inventory file is a list of servers (also called hosts) that Ansible will manage. For example, if we have two AWS instances and want to install Apache on both, we define these instances in an inventory file by specifying their IP addresses, usernames, and authentication details (e.g., SSH key or password). Ansible uses this file to connect to the servers and execute tasks.**

## Examples of inventory files

### 1. Inventory file to ping one server.

- Firstly install `sshpass` in the other server.

- Now create a file `inventory.txt`.

```yaml
server1 ansible_host=192.168.1.36 ansible_user=goldy ansible_ssh_pass=$*
```
- `server1` is a alias of system which we are going to ping.

- Enter your credentials and save the file then run this command.

```bash
ansible -i inventory.txt server1 -m ping
```

- `-i` for telling ansible our inventory file is here, if we don't provide this then ansible will take default inventory file from `/etc/ansible/hosts`, `-m` for module, `ping` is module name. I will discuss about modules in next chapters.

- After runing the command your output will be

```bash
server1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
```

- `"ping": "pong"` means ping is **DONE**.

### 2. Now ping an **ec2 instance.**

- Create a Invetory file for ec2 Instance.

```yaml
ec2_instance ansible_host=52.15.186.61 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
```

- Now run this command.

```bash
ansible -i inventory_ec2.txt ec2_instance -m ping
```

- Your output will be like this.

```bash
ec2_instance | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

### 3. Now we will create a inventory file of servers.

- Create a file.

```yaml
webserver ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver ansible_host=52.15.186.61 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
```

- We add two servers in ansible inventory now we ping these servers one by one.

```bash
ansible -i inventory_servers.txt webserver -m ping
```

```bash
ansible -i inventory_servers.txt sqlserver -m ping
```

- If we want to ping all the servers in our inventory file then run.

```bash
ansible -i inventory_servers.txt all -m ping 
```

### 4. If we have larger number of servers in inventory file.

- This is my inventory file.

```yaml
webserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver2 ansible_host=52.15.186.61 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver4 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver5 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver2 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver4 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver5 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
```

- Some of these are dummy entries.

- Now if want to ping all the server then i can run

```bash
ansible -i inventory_all_servers.txt all -m ping
```

- Now if i want to ping only sql servers then

```bash
ansible -i inventory_servers.txt sqlserver* -m ping
```

- Now the output is

```bash
sqlserver1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
sqlserver4 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
sqlserver2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
sqlserver5 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
sqlserver3 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

- Same if i want to ping webservers only then.

```bash
ansible -i inventory_servers.txt webserver* -m ping
```

### 5. Create groups of serers.

- Suppose i have this inventory file.

```yaml
apacheserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
nginxserver2 ansible_host=52.15.186.61 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver4 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver5 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
mongodbserver2 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
postgresserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
```
- Now i have to ping these database servers `sqlserver1`, `mongodbserver2`, `postgresserver3` then i will create group of these three servers.

- Now my inventory file looks like.

```yaml
apacheserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
nginxserver2 ansible_host=52.15.186.61 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver4 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver5 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
mongodbserver2 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
postgresserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem

[database_servers]
sqlserver1
mongodbserver2
postgresserver3
```

- Now run the command.

```bash
ansible -i inventory_servers.txt database_servers -m ping
```

- Other method to create a groups.

```yaml
[web_servers]
webservers[1:5]
```

- It includes webservers from 1 to 5.

### 6. Group of Groups.

- This the inventory file.

```yaml
webserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver2 ansible_host=52.15.186.61 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver4 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
webserver5 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver1 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver2 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver3 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver4 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
sqlserver5 ansible_host=18.221.38.126 ansible_user=ubuntu ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem

[web_servers]
webserver[1:5]

[sql_servers]
sqlserver[1:5]x

[allservers:children]
web_servers
sql_servers
```

- Look at below `[allservers:children]`, `allservers` is a group of `web_servers`, `sql_servers` these two groups. If we have to create group of group then we use `:children` at the last of group name.

- Command.

```bash
ansible -i inventory_servers.txt allservers -m ping
```

## Other things which we can add in our inventory file.

- `ansible_connection=ssh`
- `ansible_port=22`