FROM elasticsearch:5.2.2

WORKDIR /usr/share/elasticsearch

COPY elasticsearch.yml config/elasticsearch.yml
COPY logging.yml config/logging.yml
COPY elasticsearch-entrypoint.sh /docker-entrypoint.sh

RUN bin/elasticsearch-plugin install discovery-ec2
