FROM ubuntu:10.04

RUN groupadd ansible && useradd -g ansible -m ansible

ENV AUTH_KEY='ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAIEAlaxYCKgDC9DCTz8sAeIsBIlfc2oKAw/XmPl2Qa5vO2iqpsSaRYUAfLsRrFx+xAMzFge3eiNSk36EdCFJxOjc58qc7313iP2Bon41NkUxZlrfeCGrhcZJ1tAixF3UTfeXhnkw2dZ2rFBM/HmVP2xVYmV85pTAp6VksIbjRIn9GR8= olivier@PowerBook.local'

RUN apt-get update; \
	DEBIAN_FRONTEND=noninteractive apt-get install -y openssh-server; \
	DEBIAN_FRONTEND=noninteractive apt-get install -y python python-simplejson python-mysqldb mysql-client;

RUN mkdir /var/run/sshd
RUN echo 'root:screencast' | chpasswd

RUN mkdir -p /home/ansible/.ssh; \
	echo $AUTH_KEY > /home/ansible/.ssh/authorized_keys; \
	chown -R ansible:ansible /home/ansible/.ssh; \
	chmod 700 /home/ansible/.ssh; \
	chmod 600 /home/ansible/.ssh/authorized_keys;

#RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
