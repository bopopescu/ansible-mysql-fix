FROM ubuntu:10.04

RUN groupadd -r mysql && useradd -r -g mysql mysql

ENV PATH=/usr/local/mysql/bin:$PATH

RUN sed -i -e 's/archive.ubuntu.com\|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive \
	apt-get install -y libdbi-perl libdbd-mysql-perl psmisc libwrap0 libaio1 openssl

RUN mkdir -p /var/lib/mysql; \
	chown -R mysql /var/lib/mysql

ADD install/mariadb-5.5.50-linux-x86_64.tar.gz /usr/local/

RUN ln -s /usr/local/mariadb-5.5.50-linux-x86_64 /usr/local/mysql

WORKDIR /usr/local/mysql

RUN ./scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/var/lib/mysql --user=mysql; \
	chown -R root .;

RUN { \
		echo '[mysqld]'; \
		echo 'port = 3306'; \
		echo 'socket = /tmp/mysql.sock'; \
		echo 'skip-host-cache'; \
		echo 'skip-name-resolve'; \
		echo 'datadir = /var/lib/mysql'; \
	} > /etc/mysql/my.cnf

VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/docker-entrypoint.sh

EXPOSE 3306

USER mysql

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
CMD /usr/local/mysql/bin/mysqld_safe --user=mysql
