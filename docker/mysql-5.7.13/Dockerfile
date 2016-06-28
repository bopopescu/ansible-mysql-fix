FROM ubuntu:10.04

RUN groupadd -r mysql && useradd -r -g mysql mysql

COPY install /install

ENV PATH=/usr/local/mysql/bin:$PATH

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libdbi-perl libdbd-mysql-perl psmisc libwrap0 libaio1

RUN mkdir -p /usr/local; \
	mkdir -p /var/lib/mysql; \
	chown -R mysql /var/lib/mysql

WORKDIR /usr/local

RUN tar -zxpf /install/mysql-5.7.13-linux-glibc2.5-x86_64.tar.gz; \
	ln -s mysql-5.7.13-linux-glibc2.5-x86_64 mysql

WORKDIR /usr/local/mysql

RUN { \
		echo '[mysqld]'; \
		echo 'port = 3306'; \
		echo 'socket = /tmp/mysql.sock'; \
		echo 'skip-host-cache'; \
		echo 'skip-name-resolve'; \
		echo 'datadir = /var/lib/mysql'; \
	} > /etc/mysql/my.cnf

#RUN ./bin/mysqld --initialize-insecure --user=mysql; \
RUN chown -R root .;

VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]

EXPOSE 3306

CMD /usr/local/mysql/bin/mysqld_safe --user=mysql
