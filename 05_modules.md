# Modules

**In Ansible, modules are like small tools that do specific tasks, such as installing software, copying files, or managing services. They make it easy to automate tasks without writing complex code. Think of them as building blocks for your playbooks!**

## Let's do somethings with ansible modules :-

### 1. Copy files from host machine to server.

- Firstly create inventory for the server or servers thn create playbook like this.

```yaml
- name: copy a file
  hosts: webserver1
  tasks:
    - name: copy file to home
      copy: src=test.txt dest=/tmp/goldy.txt
```

- We can add other arguments like owner permmisions and groups.

```yaml
- name: copy a file
  hosts: webserver1
  become: yes
  tasks:
    - name: copy file to home
      copy: src=test.txt dest=/tmp/bawa.txt owner=goldy group=goldy mode=0644
```

- `become: yes` is for having permmision of root.

### 2. Add something in file

- Suppose you have to add one line in a file of server and you don't want it's entry should we duplicate in the file then we can use `Lineinfile` module to ignore the content if already exist in the file.

- Playbook 

```yaml
- name: learning fileinline
  hosts: webserver1
  tasks:
    - name: adding my name in the record
      lineinfile: path=/home/ubuntu/record.txt line=goldy
```

- This playbook will never write `goldy` if goldy is already writen in the `record.txt`.

### 3. Script module.

- We use script module if we have to run any script on the remote server.

```yaml
- name: learning script module
  hosts: webserver1
  tasks:
    - name: creating a test directory
      script: test.sh
```

- `test.sh` is my script which have the only one line `mkdir /home/ubuntu/test`

- Now we will add condition in this playbook. If `test` directory already exists then don't run the script.

```yaml
- name: learning script module
  hosts: webserver1
  tasks:
    - name: creating a test directory
      script: test.sh creates=/home/ubuntu/test 
```

- Now if `/home/ubuntu/test` this path exist not remote machine then ansible will not run our script.

```yaml
- name: learning script module
  hosts: webserver1
  tasks:
    - name: creating a test directory
      script: test.sh removes=/home/ubuntu/test 
```

- with this `remove` argument the script will run only if `/home/ubuntu/test` exits.

### 4. Service module.

- service module is used to manage services like apache and nginx.

```yaml
- name: start httpd
  hosts: webserver1
  become: yes
  tasks:
    - name: start httpd
      service:
        name: apache2
        state: started
```

- Now this playbook will start `apache2`.

- If want to stop then change `started` with `stoped`.

### 5. User module.

- We use this module to manage users on the remote servers.

```yaml
- name: learning about user module
  hosts: webserver1
  become: yes
  tasks:
    - name: creating an user on webserver1
      user: name="goldy1" state=present password="${Your hashed password}"
```

- Convert your password to hash then add in the playbook.

- To convert password to hash use this command.

```bash
python3 -c 'import crypt; print(crypt.crypt("YOUR_PASSWORD", crypt.mksalt(crypt.METHOD_SHA512)))'
```

- To delete any user then change state `present` to `absent`.

```yaml
- name: learning about user module
  hosts: webserver1
  become: yes
  tasks:
    - name: creating an user on webserver1
      user: name="goldy1" state=absent
```

- Note :- This will not remove the home directory of the user if you want to delete home directory of the user then add `remove=yes`.

```yaml
- name: learning about user module
  hosts: webserver1
  become: yes
  tasks:
    - name: creating an user on webserver1
      user: name="goldy" state=absent remove=yes
```

### 6. Apt module.

- We use apt module to manage packages on the remote server.

- to install any package.

```yaml
- name: Install a package
  hosts: webserver1
  become: yes
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
```

- To install multiple packages.

```yaml
- name: Install multiple packages
  hosts: all
  become: yes
  tasks:
    - name: Install multiple packages
      apt:
        name:
          - git
          - curl
          - vim
        state: present
```

- To install latest package.

```yaml
- name: Ensure latest version of nginx is installed
  hosts: webservers
  become: yes
  tasks:
    - name: Update package cache and install latest nginx
      apt:
        name: nginx
        state: latest
        update_cache: yes
```

- `update_cache: yes` Updates the package cache before performing any operations.

- To delete any package.

```yaml
- name: Remove a package
  hosts: all
  become: yes
  tasks:
    - name: Remove Apache
      apt:
        name: apache2
        state: absent
```

- To update the system.

```yaml
- name: Update and upgrade the system
  hosts: all
  become: yes
  tasks:
    - name: Update package cache
      apt:
        update_cache: yes

    - name: Upgrade all packages
      apt:
        upgrade: dist
```

- `upgrade: dist`  It not only upgrades installed packages to their latest versions but also intelligently handles package dependencies. It can install new packages or remove existing ones if required to satisfy dependencies.

- To add any repository then install the package.

```yaml
- name: Add a repository and install a package
  hosts: all
  become: yes
  tasks:
    - name: Add a new repository
      apt_repository:
        repo: ${REPO_NAME}

    - name: Install Python 3.9
      apt:
        name: ${PACKAGE_NAME}
        state: present
```

