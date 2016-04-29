FROM java:8-jre

# install plugin dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
		libzmq3 \
	&& rm -rf /var/lib/apt/lists/*

# the "ffi-rzmq-core" gem is very picky about where it looks for libzmq.so
RUN mkdir -p /usr/local/lib \
	&& ln -s /usr/lib/*/libzmq.so.3 /usr/local/lib/libzmq.so

# https://www.elastic.co/guide/en/logstash/2.3/package-repositories.html
# https://packages.elastic.co/GPG-KEY-elasticsearch
RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4

ENV LOGSTASH_MAJOR 2.3
ENV LOGSTASH_VERSION 1:2.3.2-1

RUN echo "deb http://packages.elastic.co/logstash/${LOGSTASH_MAJOR}/debian stable main" > /etc/apt/sources.list.d/logstash.list

RUN set -x \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends logstash=$LOGSTASH_VERSION \
	&& rm -rf /var/lib/apt/lists/*

ENV PATH /opt/logstash/bin:$PATH

COPY docker-entrypoint.sh /

USER 1000

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["logstash", "agent"]