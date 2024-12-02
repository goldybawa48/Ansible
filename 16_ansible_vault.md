# Ansible Vault

**We use ansible vault to encrypt our plan text inventory file.**

### 1. How to encrypt :-

- `ansible-vault` command is used to encrypt inventory file.

- Suppose my inventory file looks like this.

```txt
[webserver1]
ansible_host=192.168.25.15 ansible_ssh_pass=password ansible_connection=ssh ansible_port=22 ansible_user=root

[sqlserver1]
ansible_host=192.168.25.16 ansible_ssh_pass=password ansible_connection=ssh ansible_port=22 ansible_user=root

[webservers]
webserver1

[databaseservers]
sqlserver1

[web_database_servers]
webserver1
sqlserver1
```

- Now if save this file on the server anyone can read the password from this file.

- Now encrypt this file run this command.

```bash
ansible-vault encrypt inventory.txt --output encrypted.txt
```

- After runing this command give your password to read the file without encription.

- Now read the encrypted file with `cat` command. 

- After that delete plan text file.

### 2. How to edit something in encrypted file

- After deleting the plan text file of inventory you have only encrypted one then if you have to change anything in that file how you can change ??

- To change something in encrypted file run

```bash
 ansible-vault edit encrypted.txt 
 ```

 ### 3. How to read the encrypted file

 - To read only your encrypted file with plan text.

 ```bash
ansible-vault view encrypted.txt
```