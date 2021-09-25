FROM perl:5.34.0-slim-buster

# datasource=git-tags depName=nabe-abk/adiary versioning=docker
ARG ADIARY_VERSION="v3.34"

MAINTAINER yanorei32 <yanorei@hotmail.co.jp>
 
RUN set -ex; \
	apt-get update -qq; \
	apt-get install -qq -y --no-install-recommends \
		perlmagick libnet-ssleay-perl libcryptx-perl libfcgi-perl; \
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get install -qq -y --no-install-recommends wget ca-certificates; \
	wget -q -O adiary.tar.gz https://github.com/nabe-abk/adiary/archive/refs/tags/${ADIARY_VERSION}.tar.gz; \
	tar xf adiary.tar.gz; \
	mv adiary-$(echo ${ADIARY_VERSION} | sed s/v//) adiary; \
	rm adiary.tar.gz; \
	apt-mark auto '.*' > /dev/null; \
	[ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*; \
	mkdir -p /adiary/shared/; \
	mv /adiary/pub/ /adiary/shared/; \
	mv /adiary/pub-dist/ /adiary/shared/; \
	mv /adiary/theme/ /adiary/shared/; \
	mv /adiary/js/ /adiary/shared/; \
	mkdir -p /adiary/private/; \
	mv /adiary/data/ /adiary/private/; \
	mv /adiary/plugin/ /adiary/private/; \
	mv /adiary/skel.local/ /adiary/private/; \
	cat /adiary/adiary.conf.cgi.sample \
		| sed 's:pub/:shared/pub/:' \
		| sed 's:pub-dist/:shared/pub-dist/:' \
		| sed 's:theme/:shared/theme/:' \
		| sed 's:js/:shared/js/:' \
		| sed 's:data/:private/data/:' \
		| sed 's:plugin/:private/plugin/:' \
		| sed 's:skel.local/:private/skel.local/:' \
		> /adiary/adiary.conf.cgi;

VOLUME /adiary/shared/ /adiary/private/

EXPOSE 9000
CMD /adiary/adiary.fcgi 0.0.0.0:9000
