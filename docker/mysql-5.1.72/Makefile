TAG := 5.1.72
PORT := 3315
URL := http://cdn.mysql.com/archives/mysql-5.1/mysql-5.1.72-linux-x86_64-glibc23.tar.gz

.PHONY: build it run download

build:
	docker build -t ansible_mysql_user/mysql:$(TAG) .

it:
	docker run -it --rm -p $(PORT):3306 --entrypoint=/bin/bash ansible_mysql_user/mysql:$(TAG)

run:
	docker run -d -p $(PORT):3306 ansible_mysql_user/mysql:$(TAG)

download:
	mkdir -p install
	cd install && wget $(URL)
