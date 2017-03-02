FROM elasticsearch:5.2.2

WORKDIR /usr/share/elasticsearch

ADD elasticsearch.yml config/elasticsearch.yml
ADD logging.yml config/logging.yml
ADD elasticsearch-entrypoint.sh /docker-entrypoint.sh

RUN bin/elasticsearch-plugin install discovery-ec2

USER root
RUN chown elasticsearch:elasticsearch config/elasticsearch.yml
RUN chown elasticsearch:elasticsearch config/logging.yml
USER elasticsearch
