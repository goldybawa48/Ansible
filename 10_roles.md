# Roles

**We use Ansible roles to organize tasks, variables, and files into reusable and structured folders. This makes playbooks easier to manage, read, and share. Roles save time and ensure consistency when configuring multiple servers or projects.**

- Structure of role.

```python
roles/
  my_role/
    tasks/
      main.yml      # The main file for tasks.
    vars/
      main.yml      # Variables to use in tasks.
    templates/
      # Files with placeholders (Jinja2 templates).
    files/
      # Static files to be copied.
    handlers/
      main.yml      # Handlers to restart services.
    meta/
      main.yml      # Role metadata (dependencies, etc.).
    defaults/
      main.yml      # Default variables.
```

### 1. Tasks/

- This folder contains all the tasks that the role will execute, such as installing software, starting services, or copying files.

### 2. vars/

- This folder holds variables that you define for the role. These variables are generally hard-coded and should not change during execution.

- `vars/main.yml`

```yaml
apache_port: 8080
document_root: /var/www/html
```

- `tasks/main.yml`

```yml
- name: Configure Apache
  lineinfile:
    path: /etc/apache2/ports.conf
    line: "Listen {{ apache_port }}"
```

### 3. templates/

- This folder is for Jinja2 template files. Templates are files with placeholders ({{ variable }}) that get replaced with real values at runtime.

- `templates/nginx.conf.j2`

```yml
server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
}
```

- use. `tasks/main.yml`

```yml
- name: Deploy NGINX config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```

### 4. files/

- This folder stores static files (e.g., scripts, configuration files, or binaries) that are directly copied to the target system.4

- `files/index.html`

```hmtl
<h1>Welcome to Goldy's Website!</h1>
```

- Usage `tasks/main.yml`

```yml
- name: Copy welcome page
  copy:
    src: index.html
    dest: /var/www/html/index.html
```

### 5. handlers/

- Handlers are special tasks that trigger only when notified by other tasks. They are often used to restart services after configuration changes.

- example `handlers/main.yml`

```yml
- name: Restart Apache
  service:
    name: apache2
    state: restarted
```

- Usage `tasks/main.yml`

```yml
- name: Update Apache config
  copy:
    src: apache.conf
    dest: /etc/apache2/apache2.conf
  notify:
    - Restart Apache
```

### 6. meta/

- This folder contains metadata about the role, like its dependencies or compatibility with Ansible versions.

```yml
dependencies:
  - { role: common, tags: ['common'] }
```

### 7. defaults/

- This folder holds default variables for the role. These variables can be overridden by higher precedence variables (e.g., in playbooks or extra vars).

- Example `defaults/main.yml`

```yml
apache_port: 80
document_root: /var/www/html
```

