TAG := 5.2.11
PORT := 3308
URL := http://archive.mariadb.org//mariadb-5.2.11/kvm-bintar-hardy-amd64/mariadb-5.2.11-Linux-x86_64.tar.gz

.PHONY: build it run download

build:
	docker build -t ansible_mysql_user/mariadb:$(TAG) .

it:
	docker run -it --rm -p $(PORT):3306 --entrypoint=/bin/bash ansible_mysql_user/mariadb:$(TAG)

run:
	docker run -d -p $(PORT):3306 ansible_mysql_user/mariadb:$(TAG)

download:
	mkdir -p install
	cd install && wget $(URL)
