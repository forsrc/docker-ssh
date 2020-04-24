FROM centos:7

# who is the author
MAINTAINER forsrc

ENV USER=forsrc
ARG PASSWD=forsrc
RUN yum install -y sudo
RUN useradd -m --shell /bin/bash $USER && \
    echo "$USER:$PASSWD" | chpasswd && \
    echo "$USER ALL=(ALL) ALL" >> /etc/sudoers


# install openssh
RUN yum -y  install openssh-server openssh-clients

RUN yum clean all && rm -rf /var/cache/yum/*

RUN sed -ri 's/^#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/^UsePAM yes/UsePAM no/' /etc/ssh/sshd_config

#generate key files
RUN ssh-keygen -q -t rsa   -b 2048 -f /etc/ssh/ssh_host_rsa_key      -N ''
RUN ssh-keygen -q -t ecdsa         -f /etc/ssh/ssh_host_ecdsa_key    -N ''
RUN ssh-keygen -q -t dsa           -f /etc/ssh/ssh_host_ed25519_key  -N ''

# login localhost without password
RUN mkdir /home/$USER/.ssh
RUN ssh-keygen -f /home/$USER/.ssh/id_rsa -N ''
RUN touch /home/$USER/.ssh/authorized_keys
RUN cat /home/$USER/.ssh/id_rsa.pub >> /home/$USER/.ssh/authorized_keys


# open the port 22
EXPOSE 22

# when start a container it will be executed
CMD ["/usr/sbin/sshd","-D"]
