TAG := 5.2.11
PORT := 3308
# http://archive.mariadb.org//mariadb-5.2.11/kvm-bintar-hardy-amd64/mariadb-5.2.11-Linux-x86_64.tar.gz

build:
	docker build -t ansible_mysql_user/mariadb:$(TAG) .

it:
	docker run -it --rm -p $(PORT):3306 --entrypoint=/bin/bash ansible_mysql_user/mariadb:$(TAG)

run:
	docker run -d -p $(PORT):3306 ansible_mysql_user/mariadb:$(TAG)
