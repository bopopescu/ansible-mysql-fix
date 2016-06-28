TAG := 5.6.31
PORT := 3318
URL := http://cdn.mysql.com//Downloads/MySQL-5.6/mysql-5.6.31-linux-glibc2.5-x86_64.tar.gz

build:
	docker build -t ansible_mysql_user/mysql:$(TAG) .

it:
	docker run -it --rm -p $(PORT):3306 --entrypoint=/bin/bash ansible_mysql_user/mysql:$(TAG)

run:
	docker run -d -p $(PORT):3306 ansible_mysql_user/mysql:$(TAG)

download:
	mkdir -p install
	cd install && wget $(URL)
