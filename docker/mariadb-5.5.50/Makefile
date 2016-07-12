TAG := 5.5.50
PORT := 3313
URL := http://ftp.eenet.ee/pub/mariadb//mariadb-5.5.50/bintar-linux-x86_64/mariadb-5.5.50-linux-x86_64.tar.gz

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
