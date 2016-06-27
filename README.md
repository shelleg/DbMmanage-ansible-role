RDS-Andible-Role
================

This role is designed to act as RDS which means it serves all the services requiring a datanase scheme, user, access rights etc.

Requirements
------------

Considering you have MySql/MariaDB/Postgresql etc this role will create install the database / alternatively will connect to a remote database server and create the scheme/user/etc.

Role Variables
--------------

db_mode: local || remote - when local all schemas / users etc will assume a database is listening 
db_flavor: mysql || postgresql || rds || other - when mysql mysql manager will work against mysql ans the same for other engines.

**Please note** this is the first version and will most defintelly get refactored in the future.


Dependencies
------------


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: rds-ansible-role, db_mode: remote, db_flavor: rds }
				 - { role: <i need a db + user > } 

TODO: add support for ansible vault

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
