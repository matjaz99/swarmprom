FROM prom/node-exporter:v1.1.1

ENV NODE_ID=none

USER root

COPY conf /etc/node-exporter/

RUN mkdir -p /file-collector

ENTRYPOINT  [ "/etc/node-exporter/docker-entrypoint.sh" ]
CMD [ "/bin/node_exporter" ]
