dbmanage Ansible role
======================

This role is designed to act as a all-in-one database operations role which means it serves all the services requiring a 
database scheme, user, access rights etc.

Using this without any overrides will result in Mariadb running locally with user `root` and no password ... 
so if you don't want anything from the role but a databse installed and takje it from there so you can do tha too ;)

Requirements
------------

**Ansible >= 2.1**

Considering you have MySql/MariaDB/Postgresql etc this role will create install the database / alternatively will connect to a remote database server and create the scheme/user/etc.

Role Variables
--------------

db_mode: `local` (default mode) || `remote` - when local all schemas / users etc will assume a database is avail on `local` via sock or `remote` via tcp. 
db_flavor: `mariadb` (default flavor) || `mysql` || `postgresql` || `rds` || `other` - when mysql  manager will work against mysql and the same for other engines.

**Please note** this is the first version and will most definitely get refactored in the future.

Dependencies
------------
* none ATM

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: dbmanage-ansible-role, db_mode: remote, db_flavor: rds }
	     - { role: artifactory-ansible-role } 
	     - { role: jenkins-ansible-role } 

or for more options:

    - hosts: servers
      roles:
         - { role: dbmanage-ansible-role }
	     - { role: sonar-ansible-role } 

TODO: add support for ansible vault

Supported OS's
--------------
Tested with **Centos 7** and **Ubuntu 160.4** see vagrant boxes used.

Testing
-------

`vagrant up` will instantiate 4 vm's via Virtualbox:

* centos7 - for latest centos release [ Currently 16.04 ]
* ubuntu1604 - for latest ubuntu release [ Currently 7 ]
* centos6 - for centos 6
* ubuntu1404 - for ubuntu 14.05

Considering I am not 100% sure yet on the testing strategy I went along and performed the following:
see test/test.yml for the full file but basically doing the following seemed to suffice ATM:

* note to self check why defaults/main.yml ins't picked up automatically - see tests/test.yml, curentelly the work arround is as follows:

```python
pre_tasks:
- include_vars: ../defaults/main.yml
```  

* running tests:

`vagrant up centos6` (vagrant up will run all nodes this is just an example) will yield:

```python
vagrant provision centos6
==> centos6: Running provisioner: ansible...
    centos6: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [setup] *******************************************************************
ok: [centos6]

TASK [include_vars] ************************************************************
ok: [centos6]

TASK [Create a my.cnf file in order to test mysql cli connectivity] ************
changed: [centos6]

TASK [Run some sql command and return exist status] ****************************
changed: [centos6]

TASK [cleanup my.cnf] **********************************************************
changed: [centos6]

PLAY RECAP *********************************************************************
centos6                    : ok=5    changed=3    unreachable=0    failed=0   
```

* The "test"
Considering the `mariadb` which is the default installation flavor alongside the default `local` mode with no special args we:
1. create a /root/.my.cnf file
2. Run an arbitery sql command in our case `mysql -e show databases;` - if this is successful we are good to go .. 
3. Remove the /root/.my.cnf file so no hacker has access to the root passowrd 

* Only run tests:
 see Vagrantfile line 38 `ansible.raw_arguments = ["-tags test"]` this is somthing you can pass to Vagrnat / Ansible provisioner to limit only to tag test.
 This isn't default considering it requires the playbook to run before the test are triggered.
 
 so now vagrant provision will yield:
 
```python
==> centos7: Running provisioner: ansible...
    centos7: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [setup] *******************************************************************
ok: [centos7]

TASK [include_vars] ************************************************************
ok: [centos7]

TASK [Create a my.cnf file in order to test mysql cli connectivity] ************
changed: [centos7]

TASK [Run some sql command and return exist status] ****************************
changed: [centos7]

TASK [assert] ******************************************************************
ok: [centos7]

TASK [debug command_resut output...] *******************************************
ok: [centos7] => {
    "command_result": {
        "changed": true, 
        "cmd": "mysql -e 'show databases;'", 
        "delta": "0:00:00.008122", 
        "end": "2016-07-03 04:37:09.826421", 
        "rc": 0, 
        "start": "2016-07-03 04:37:09.818299", 
        "stderr": "", 
        "stdout": "Database\ninformation_schema\nmysql\nperformance_schema\ntest", 
        "stdout_lines": [
            "Database", 
            "information_schema", 
            "mysql", 
            "performance_schema", 
            "test"
        ], 
        "warnings": []
    }
}

TASK [cleanup my.cnf] **********************************************************
changed: [centos7]

PLAY RECAP *********************************************************************
centos7                    : ok=7    changed=3    unreachable=0    failed=0   

==> centos6: Running provisioner: ansible...
    centos6: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [setup] *******************************************************************
ok: [centos6]

TASK [include_vars] ************************************************************
ok: [centos6]

TASK [Create a my.cnf file in order to test mysql cli connectivity] ************
changed: [centos6]

TASK [Run some sql command and return exist status] ****************************
changed: [centos6]

TASK [assert] ******************************************************************
ok: [centos6]

TASK [debug command_resut output...] *******************************************
ok: [centos6] => {
    "command_result": {
        "changed": true, 
        "cmd": "mysql -e 'show databases;'", 
        "delta": "0:00:00.009028", 
        "end": "2016-07-03 08:37:14.633373", 
        "rc": 0, 
        "start": "2016-07-03 08:37:14.624345", 
        "stderr": "", 
        "stdout": "Database\ninformation_schema\nmysql\nperformance_schema\ntest", 
        "stdout_lines": [
            "Database", 
            "information_schema", 
            "mysql", 
            "performance_schema", 
            "test"
        ], 
        "warnings": []
    }
}

TASK [cleanup my.cnf] **********************************************************
changed: [centos6]

PLAY RECAP *********************************************************************
centos6                    : ok=7    changed=3    unreachable=0    failed=0   

==> ubutnu1604: Running provisioner: ansible...
    ubutnu1604: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [setup] *******************************************************************
ok: [ubutnu1604]

TASK [include_vars] ************************************************************
ok: [ubutnu1604]

TASK [Create a my.cnf file in order to test mysql cli connectivity] ************
changed: [ubutnu1604]

TASK [Run some sql command and return exist status] ****************************
changed: [ubutnu1604]

TASK [assert] ******************************************************************
ok: [ubutnu1604]

TASK [debug command_resut output...] *******************************************
ok: [ubutnu1604] => {
    "command_result": {
        "changed": true, 
        "cmd": "mysql -e 'show databases;'", 
        "delta": "0:00:00.018395", 
        "end": "2016-07-03 08:37:20.319579", 
        "rc": 0, 
        "start": "2016-07-03 08:37:20.301184", 
        "stderr": "", 
        "stdout": "Database\ninformation_schema\nmysql\nperformance_schema", 
        "stdout_lines": [
            "Database", 
            "information_schema", 
            "mysql", 
            "performance_schema"
        ], 
        "warnings": []
    }
}

TASK [cleanup my.cnf] **********************************************************
changed: [ubutnu1604]

PLAY RECAP *********************************************************************
ubutnu1604                 : ok=7    changed=3    unreachable=0    failed=0   

==> ubuntu1404: Running provisioner: ansible...
    ubuntu1404: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [setup] *******************************************************************
ok: [ubuntu1404]

TASK [include_vars] ************************************************************
ok: [ubuntu1404]

TASK [Create a my.cnf file in order to test mysql cli connectivity] ************
changed: [ubuntu1404]

TASK [Run some sql command and return exist status] ****************************
changed: [ubuntu1404]

TASK [assert] ******************************************************************
ok: [ubuntu1404]

TASK [debug command_resut output...] *******************************************
ok: [ubuntu1404] => {
    "command_result": {
        "changed": true, 
        "cmd": "mysql -e 'show databases;'", 
        "delta": "0:00:00.013057", 
        "end": "2016-07-03 08:37:28.179577", 
        "rc": 0, 
        "start": "2016-07-03 08:37:28.166520", 
        "stderr": "", 
        "stdout": "Database\ninformation_schema\nmysql\nperformance_schema", 
        "stdout_lines": [
            "Database", 
            "information_schema", 
            "mysql", 
            "performance_schema"
        ], 
        "warnings": []
    }
}

TASK [cleanup my.cnf] **********************************************************
changed: [ubuntu1404]

PLAY RECAP *********************************************************************
ubuntu1404                 : ok=7    changed=3    unreachable=0    failed=0   
```
 
 NOTE to self: check how to do this more elegantly.
 

Thanks to
---------
* Mariadb       :: I copied patterns from the [pcextreme mardiadb role](https://github.com/PCextreme/ansible-role-mariadb) - couldn't just re-use it :(
* MySql         ::
* Postgresql    :: 
* Other         ::

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
