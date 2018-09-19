FROM debian:jessie

# add our user and group first to make sure their IDs get assigned consistently
RUN groupadd -r kibana && useradd -r -m -g kibana kibana

RUN apt-get update && apt-get install -y \
                ca-certificates \
                wget \
        --no-install-recommends && rm -rf /var/lib/apt/lists/*

# https://www.elastic.co/guide/en/kibana/4.4/setup.html#kibana-apt
# https://packages.elasticsearch.org/GPG-KEY-elasticsearch
#RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4
RUN wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -

ENV KIBANA_MAJOR 6.4
ENV KIBANA_VERSION 6.4.0

RUN apt-get update && apt-get -y install apt-transport-https curl

RUN echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list

RUN set -x \
        && apt-get update \
        && apt-get install -y --no-install-recommends kibana=$KIBANA_VERSION \
        && chown -R kibana:kibana /usr/share/kibana \
        && chown -R kibana:kibana /var/lib/kibana

ENV PATH /usr/share/kibana/bin:$PATH

#COPY docker-entrypoint.sh /

EXPOSE 5601

RUN chmod -R og+w /usr/share/kibana

RUN chmod -R og+w /var/lib/kibana

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["kibana"]

USER 1000
