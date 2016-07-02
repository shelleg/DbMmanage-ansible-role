DbMmanage Ansible role
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
         - { role: rds-ansible-role, db_mode: remote, db_flavor: rds }
	     - { role: artifactory-ansible-role } 
	     - { role: jenkins-ansible-role } 

or for more options:

    - hosts: servers
      roles:
         - { role: rds-ansible-role }
	     - { role: sonar-ansible-role } 

TODO: add support for ansible vault

Thanks to
---------
* Mariadb   :: I copied patterns from the [pcextreme mardiadb role](https://github.com/PCextreme/ansible-role-mariadb) - couldn't just re-use it :(
* MuSql     ::
* Postgres  :: 
* Other     ::

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
