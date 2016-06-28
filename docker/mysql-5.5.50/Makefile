TAG := 5.5.50
PORT := 3317
URL := http://cdn.mysql.com//Downloads/MySQL-5.5/mysql-5.5.50-linux2.6-x86_64.tar.gz

build:
	docker build -t ansible_mysql_user/mysql:$(TAG) .

it:
	docker run -it --rm -p $(PORT):3306 --entrypoint=/bin/bash ansible_mysql_user/mysql:$(TAG)

run:
	docker run -d -p $(PORT):3306 ansible_mysql_user/mysql:$(TAG)

download:
	mkdir -p install
	cd install && wget $(URL)
