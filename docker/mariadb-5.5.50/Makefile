TAG := 5.5.50
PORT := 3313
# http://ftp.eenet.ee/pub/mariadb//mariadb-5.5.50/bintar-linux-x86_64/mariadb-5.5.50-linux-x86_64.tar.gz

build:
	docker build -t ansible_mysql_user/mariadb:$(TAG) .

it:
	docker run -it --rm -p $(PORT):3306 --entrypoint=/bin/bash ansible_mysql_user/mariadb:$(TAG)

run:
	docker run -d -p $(PORT):3306 ansible_mysql_user/mariadb:$(TAG)
