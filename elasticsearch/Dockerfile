FROM java:8-jre

# https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-repositories.html
# https://packages.elasticsearch.org/GPG-KEY-elasticsearch
RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4

ENV ELASTICSEARCH_MAJOR 6.4
ENV ELASTICSEARCH_VERSION 6.4
ENV ELASTICSEARCH_REPO_BASE https://artifacts.elastic.co/packages/6.x/apt

RUN echo "deb $ELASTICSEARCH_REPO_BASE stable main" > tee -a /etc/apt/sources.list.d/elastic-6.x.list

RUN set -x \
	&& apt-get install apt-transport-https
	&& apt-get update 
	&& apt-get install metricbeat

ENV PATH /usr/share/elasticsearch/bin:$PATH

WORKDIR /usr/share/elasticsearch

RUN set -ex \
	&& for path in \
		./data \
		./logs \
		./config \
		./config/scripts \
	; do \
		mkdir -p "$path"; \
		chown -R elasticsearch:elasticsearch "$path"; \
	done

COPY config ./config

COPY docker-entrypoint.sh /

EXPOSE 9200 9300

RUN chmod -R og+w /usr/share/elasticsearch
USER 1000

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["elasticsearch"]
