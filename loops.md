# Loops

Loops is newly added feature in yaml. It is used to iterate over a list of items.

- example 1:

```yml
# playbook-create-users1.yml
---
-
  name: Create users
  hosts: localhost
  tasks:
    - user: name='{{ item }}' state=present
      loop:
        - user1
        - user2
        - user3
        - user4
```

- example 2:

```yml
# playbook-create-users2.yml
---
-
  name: Create users
  hosts: localhost
  tasks:
    - user: name={{ item.name }} uid={{ item.uid }} gid={{ item.gid }} groups={{ item.groups }} state=present
      loop:
        - name: user1 
          uid: 1001 
          gid: 1001
          groups: wheel
        - name: user2 
          uid: 1002 
          gid: 1002
          groups: users
        - name: user3
          uid: 1003
          gid: 1003
          groups: users
        - name: user4
          uid: 1004
          gid: 1004
          groups: users
```
*There are two ways to use loops. One is to use loop with a list of items. The other is to use loop with a dictionary. The difference between the two is that the first one is a list of items. The second one is a dictionary. The first one is used to iterate over a list of items. The second one is used to iterate over a dictionary. We can also use `with_*` to iterate over a list of items.*

- example 3:

```yml
# playbook-create-users3.yml
---
-
  name: Create users
  hosts: localhost
  tasks:
    - user: name='{{ item }}' state=present
      with_items:
        - user1
        - user2
        - user3
        - user4
    - debug: var=item
      with_file:
        - /etc/hosts
        - /etc/resolv.conf
        - /etc/ntp.conf
```
- example 4:

```yml
# playbook-create-urls.yml
---
-
  name: Get from multiple urls
  hosts: localhost
  tasks:
    - debug: var=item
      with_urls:
        - http://www.example.com/
        - http://www.example2.com/
        - http://www.example3.com/
```

- example 5:

```yml
# playbook-create-mogodbs.yml
---
-
  name: Check multiple mongodbs
  hosts: localhost
  tasks:
    - debug: msg="DB={{ item.database }} PID={{ item.pid }}
      with_mongodbs:
        - database: dev
          connection_string: mongodb://dev.mongo/
        - database: prod
          connection_string: mongodb://prod.mongo/
```
*Here below some of `with_*` keywords used by some lookup plugins:*
- with_items
- with_dict
- with_file
- with_url
- with_mongodb
- with_etcd
- with_openshif
- with_password
- with_rabbitmq
- with_redis

---

Exercise:

- The playbook currently runs an echo command to print a fruit name. Apply a loop directive (with_items) to the task to print all fruits defined in the fruits variable.

```yml
# playbook-print-fruits.yml
---
-
    name: 'Print list of fruits'
    hosts: localhost
    vars:
        fruits:
            - Apple
            - Banana
            - Grapes
            - Orange
    tasks:
        -
            command: 'echo "Apple"'
            # solution
            command: 'echo "{{ item }}"'
            with_items: '{{ fruits }}'`
```

- To a more realistic use case. We are attempting to install multiple packages using yum module.The current playbook installs only a single package.

```yml
# playbook-install-packages.yml
---
-
    name: 'Install required packages'
    hosts: localhost
    vars:
        packages:
            - httpd
            - binutils
            - glibc
            - ksh
            - libaio
            - libXext
            - gcc
            - make
            - sysstat
            - unixODBC
            - mongodb
            - nodejs
            - grunt
    tasks:
        -
            yum: 'name=httpd state=present'
            # solution
            yum: 'name={{ item }} state=present'
            with_items: '{{ packages }}'
```
