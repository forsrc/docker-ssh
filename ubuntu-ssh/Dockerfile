FROM ubuntu:19.10

MAINTAINER forsrc <forsrc@gmail.com>

ENV DEBIAN_FRONTEND=noninteractive
ENV USER=forsrc
ARG PASSWD=forsrc
RUN apt-get update
RUN apt-get install -y sudo
RUN useradd -m --shell /bin/bash $USER && \
    echo "$USER:$PASSWD" | chpasswd && \
    echo "$USER ALL=(ALL) ALL" >> /etc/sudoers


#RUN apt-get update
RUN apt-get install -y vim openssh-server openssh-client

RUN sed -ri 's/^#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/^UsePAM yes/UsePAM no/'                     /etc/ssh/sshd_config


# login localhost without password
RUN mkdir /home/$USER/.ssh
RUN ssh-keygen -f /home/$USER/.ssh/id_rsa -N ''
RUN touch /home/$USER/.ssh/authorized_keys
RUN cat   /home/$USER/.ssh/id_rsa.pub >> /home/$USER/.ssh/authorized_keys

RUN mkdir /var/run/sshd
RUN echo "\nStrictHostKeyChecking no\n" >> /etc/ssh/ssh_config
RUN echo "\nUserKnownHostsFile /dev/null\n" >> /etc/ssh/ssh_config


# open the port 22
EXPOSE 22


CMD ["/usr/sbin/sshd", "-D"]
