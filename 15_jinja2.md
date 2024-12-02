# Jinja 2

**In jinja 2, We only play with variables. So let's play with variables....**

### An example of playbook

- playbook

```yml
- name: learning jinja 2
  hosts: localhost
  vars:
    My_name: Goldy
    Num1:
      - 10
      - 20
      - 30
      - 40
    Num2:
      - 10
      - 40
      - 50
      - 60

  tasks:

    - debug:
        msg: "Hello {{ My_name }} Sir"

    - debug:
        msg: "Hello {{ My_name | upper }}"

    - debug:
        msg: "Hello {{ My_name | lower }}"

    - debug:
        msg: "Hello {{ My_name | replace('Goldy', 'bawa') }}"

    - debug:
        msg: "{{ Num1 | min }}"

    - debug:
        msg: "{{ Num1 | max }}"

    - debug:
        msg: "{{ Num1 | unique }}"

    - debug:
        msg: "{{ Num1 | union(Num2) }}"

    - debug:
       msg: "{{ Num1 | intersect(Num2) }}"

    - debug:
       msg: "{{ Num2 | max }}"
```

- This is very simple playbook to learn about jinja 2 syntax in ansible.