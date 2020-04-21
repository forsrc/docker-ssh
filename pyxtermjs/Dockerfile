FROM python:latest
MAINTAINER forsrc@gmail.com

ENV DEBIAN_FRONTEND=noninteractive

ENV USER=forsrc
ENV PASSWD=forsrc

RUN apt-get update

RUN apt-get install -y python3-venv pipx uwsgi python-gevent python-gevent-websocket sudo

RUN apt-get clean

RUN useradd -m $USER && \
    echo "$USER:$PASSWD" | chpasswd && \
    echo "$USER ALL=(ALL) ALL" >> /etc/sudoers

RUN echo "pipx run pyxtermjs" > /start.sh

RUN chown -R $USER:$USER /start.sh
RUN chmod +x             /start.sh

EXPOSE 5000

WORKDIR /home/$USER
USER $USER

CMD ["sh", "-c", "/start.sh"]
