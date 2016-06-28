PORT := 2210

build:
	docker build -t ansible_mysql_user/ansible .

it:
	docker run -it --rm -p $(PORT):22 --entrypoint=/bin/bash ansible_mysql_user/ansible

run:
	docker run -d -p $(PORT):22 ansible_mysql_user/ansible
