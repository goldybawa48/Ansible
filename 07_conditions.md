# Conditions

**In Ansible, conditions are used to control whether a task runs or not based on specific criteria. This makes playbooks more flexible and avoids unnecessary tasks.**

- Simple condition.

```yaml
- name: learning conditions
  hosts: webserver1
  vars:
    age: 18
  tasks:
    - name: creating file
      command: touch ~/my_age_is_18.txt
      when: age == 18
```

- In this playbook if variable `age` is equal to `18` then ansible will make an file in a homedirectory.


- A full conditional playbook.

```yaml
- name: learning conditions
  hosts: webserver1
  vars:
    age: 19
  tasks:
    - name: creating file for age 18
      command: touch ~/my_age_is_18.txt
      when: age == 18

    - name: creating file for greater then 18
      command: touch ~/my_age_greater_then_18.txt
      when: age > 18

    - name: creating file for less then 18
      command: touch ~/my_age_is_less_then_18.txt
      when: age < 18
```

- You can understand it by reading.

- `and` Example.

```yaml
- name: learning conditions
  hosts: webserver1
  vars:
    age: 11
  tasks:
    - name: creating file for age 18
      command: touch ~/my_age_is_18.txt
      when: age == 18

    - name: creating file for greater then 18
      command: touch ~/my_age_greater_then_18.txt
      when: age > 18

    - name: creating file for less then 10
      command: touch ~/my_age_is_less_then_10.txt
      when: age < 10

    - name: creating file for between 10 and 18
      command: touch ~/im_between_10_and_18.txt
      when: age > 10 and age < 18
```

- `or` example.

```yaml
- name: learning conditions
  hosts: webserver1
  vars:
    age: 11
  tasks:
    - name: creating file for age 18
      command: touch ~/my_age_is_18.txt
      when: age == 18

    - name: creating file for greater than 18
      command: touch ~/my_age_greater_then_18.txt
      when: age > 18

    - name: creating file for less than 10
      command: touch ~/my_age_is_less_then_10.txt
      when: age < 10

    - name: creating file for between 10 and 18
      command: touch ~/im_between_10_and_18.txt
      when: age > 10 and age < 18

    - name: creating file for age 11 or 12
      command: touch ~/my_age_is_11_or_12.txt
      when: age == 11 or age == 12
```