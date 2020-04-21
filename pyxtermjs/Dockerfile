FROM python:latest
MAINTAINER forsrc@gmail.com

ENV DEBIAN_FRONTEND=noninteractive

ENV USER=forsrc
ENV PASSWD=forsrc
ENV PORT=5000

RUN apt-get update

RUN apt-get install -y python3-venv pipx sudo
RUN pip install pyxtermjs uwsgi eventlet gevent gevent-websocket

RUN apt-get clean

RUN useradd -m $USER && \
    echo "$USER:$PASSWD" | chpasswd && \
    echo "$USER ALL=(ALL) ALL" >> /etc/sudoers

#ADD pyxtermjs.ini start.py /

RUN echo "import pyxtermjs.app as xterm"           >  /start.py
RUN echo "app = xterm.app"                         >> /start.py
RUN echo "app.config['cmd'] = '/bin/bash'"         >> /start.py
RUN echo "if __name__ == '__main__':"              >> /start.py
RUN echo "    xterm.socketio.run(app, port=$PORT)" >> /start.py

RUN echo "[uwsgi]"                 >  /pyxtermjs.ini
RUN echo "wsgi-file = start.py"    >> /pyxtermjs.ini
RUN echo "callable = app"          >> /pyxtermjs.ini
RUN echo ""                        >> /pyxtermjs.ini
RUN echo "http = 0.0.0.0:$PORT"    >> /pyxtermjs.ini
RUN echo "gevent = 1000"           >> /pyxtermjs.ini
RUN echo "enable-threads = true"   >> /pyxtermjs.ini
RUN echo "http-websockets = true"  >> /pyxtermjs.ini
RUN echo "master = true"           >> /pyxtermjs.ini

RUN echo "uwsgi --ini /pyxtermjs.ini" > /start.sh

RUN chown -R $USER:$USER /pyxtermjs.ini
RUN chown -R $USER:$USER /start.py
RUN chown -R $USER:$USER /start.sh
RUN chmod +x             /start.sh

EXPOSE $PORT

WORKDIR /home/$USER
USER $USER

CMD ["sh", "-c", "/start.sh"]
