# ansible-mysql-fix

Set-up to test adding support for pluggable authentication and MySQL 5.7’s new `ALTER USER` syntax to Ansible’s `mysql_user` module.

We use Docker to run different versions of MySQL and MariaDB, and a comprehensive Ansible playbook to test all of the module’s features on each version.

## Versions Tested

* **MySQL 5.0.96**: Last 5.0 release;
* **MySQL 5.1.72**: Last 5.1 release;
* **MySQL 5.5.8**: First stable 5.5 release:
	* Introduces pluggable authentication (in 5.5.7);
	* No socket authentication plugin (appears in 5.5.10);
	* Column in `mysql.user` is named `authentication_string`;
* **MySQL 5.5.50**: Last 5.5 release:
	* Socket authentication plugin is called `auth_socket` (appears in 5.5.10);
* **MySQL 5.6.31**: Last 5.6 release:
	* Introduces `sha256_password` authentication plugin;
* **MySQL 5.7.13**: Last 5.7 release:
	* Introduces `ALTER USER` syntax (in 5.7.6, first stable release is 5.7.9);
	* Introduces support for clear text passwords with pluggable authentication;
	* Removed `mysql_old_password` authentication plugin;
* **MariaDB 5.2.3**: First stable 5.2 release:
	* Introduces pluggable authentication (in 5.2.0);
	* Socket authentication plugin is called `socket_peercred`;
* **MariaDB 5.2.11**: 
	* Socket authentication plugin is renamed `unix_socket`;
* **MariaDB 5.2.14**: Last 5.2 release;
* **MariaDB 5.3.5**: First stable 5.3 release;
* **MariaDB 5.3.12**: Last 5.3 release;
* **MariaDB 5.5.23**: First stable 5.5 release:
	* `auth_string` column in `mysql.user` is renamed `authentication_string` (in 5.5.0), on par with MySQL;
* **MariaDB 5.5.50**: Last 5.5 release.

This is a work in progress. MariaDB 10+ containers will be added once the features are working in these versions.

## Starting the Containers

	cd docker
	make download # to download all the binary distribution tarballs in `mysql|mariadb-5.x.y/install`
	docker-compose up
	cd ..

## Running the Playbook

	ansible-playbook playbooks/mysql_test.yml

The playbook creates and modifies users with a variety of password types and authentication plugins. Versions that don’t supports a given feature are skipped. A normal result for the playbook is no failed tasks, and all tasks called `TASK [something: check]` are skipped:

	TASK [user_exists: check] ******************************************************
	skipping: [mariadb-5.2.11]
	skipping: [mariadb-5.2.3]
	skipping: [mariadb-5.2.14]
	skipping: [mariadb-5.3.5]
	skipping: [mariadb-5.3.12]
	skipping: [mariadb-5.5.23]
	skipping: [mysql-5.0.96]
	skipping: [mariadb-5.5.50]
	skipping: [mysql-5.1.72]
	skipping: [mysql-5.5.8]
	skipping: [mysql-5.5.50]
	skipping: [mysql-5.6.31]
	skipping: [mysql-5.7.13]

Those tasks are defined in `tests/` and are composed of a testing command, and a debug message that is skipped when the command returns the expected output:

	- name: 'user_exists: command'
	  shell: |
		mysql -u root -h {{ inventory_hostname }} -e "SELECT count(*) FROM mysql.user WHERE user = '{{ user }}' AND host = '{{ host }}'"
	  register: count
	- name: 'user_exists: check'
	  debug: 'msg="*** Failed to create user ***"'
	  when: 'count.stdout_lines[1] != "1"'

Failure results in a message being displayed:

	ok: [mariadb-5.3.5] => {
	    "msg": "*** Failed to create user ***"
	}

It is intentional not to use the `failed_when` option, in order for the playbook to run all tests without stopping on the first failure.
