FROM alpine:latest
MAINTAINER linyy2233 linyy2233@gmail.com
ENV SUPERVISOR_CONF_DIR=/etc/supervisor.d
ENV SUPERVISOR_CONF_FILE=/etc/supervisord.conf

RUN apk add --update --no-cache supervisor
RUN apk add --update --no-cache privoxy
RUN mkdir -p $SUPERVISOR_CONF_DIR /opt /data/log/supervisor/
ADD ./program /opt/program
ADD ./supervisor.d /etc/supervisor.d

ENTRYPOINT supervisord -n -c $SUPERVISOR_CONF_FILE
