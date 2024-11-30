# Let's create our first role of ansible

- Run this command to create the role directory structure.

```bash
ansible-galaxy init nginx
```

- After running this command. One directory is created which named by nginx. 

```python
nginx/
├── defaults/
│   └── main.yml       
├── files/
│   └── ...           
├── handlers/
│   └── main.yml      
├── meta/
│   └── main.yml    
├── tasks/
│   └── main.yml      
├── templates/
│   └── ...            
├── tests/
│   ├── inventory      
│   └── test.yml       
└── vars/
    └── main.yml      
```

- Now we will create the task for nginx.

- in `tasks/main.yml` write your tasks to install nginx and ensure the service running.

```yaml
---
- name: Install NGINX
  apt:
    name: nginx
    state: present
  become: true

- name: Ensure NGINX is running
  service:
    name: nginx
    state: started
    enabled: true

- name: Configure NGINX
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Restart NGINX

- name: Configure NGINX
  template:
    src: index.html
    dest: /var/www/html/index.html
  notify: Restart NGINX
```

- **Break-down :-** I'm copying my local `index` and `nginx.conf` files to the remote server.

- Now in `defaults/main.yml` write your default veriables.

```yaml
nginx_port: 80
server_host: localhost
index_file: index.html
```

- Now write configration file in `templates/nginx.conf.j2` A Jinja2 template for the NGINX configuration file.

```yaml
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 768;
}

http {
    server {
        listen {{ nginx_port }};
        server_name {{ server_host }};

        location / {
            root {{ document_root }};
            index {{ index_file }};
        }
    }
}
```

- Now create `index.html` in `templates`

```yml
<!DOCTYPE html>
<html>
<head>
    <title>Message</title>
</head>
<body>
    <div style="text-align: center; font-weight: bold;">
        Hello friends, Stay happy and healthy keep learning Ansible
    </div>
</body>
</html>
```

- Now in `handlers/main.yml` Define actions triggered by `notify`.

```yml
 ---
- name: Restart NGINX
  service:
    name: nginx
    state: restarted
```

- Now in `vars/main.yml`, Define your variables. Define variables with higher priority than defaults.

```yml
document_root: /var/www/html
```

- Now the final step get back to the folder where you runed the first command `ansible-galaxy init nginx`.

- Now Create a ansible play-book here.

```yml
---
- name: Deploy NGINX using the Role
  hosts: webserver1
  become: true
  roles:
    - nginx
```

- Then create a inventory file.

```txt
webserver1 ansible_user=ubuntu ansible_connection=ssh ansible_port=22 ansible_host=18.220.246.239 ansible_ssh_private_key_file=/home/goldy/Downloads/goldy.pem
```

- Now run the playbook and search your ip you ui looks like this.

![Screenshot from 2024-11-30 20-16-02](https://github.com/user-attachments/assets/4b177fed-bfba-4701-9f3c-f7352497610c)