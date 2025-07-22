FROM perl:5.42.0-slim-bookworm

# depName=git@github.com:nabe-abk/adiary.git
ARG ADIARY_COMMIT="f1fb910289d9be95a25c034e2024d807815dfa91"

MAINTAINER yanorei32 <yanorei@hotmail.co.jp>
 
RUN set -ex; \
	apt-get update -qq; \
	apt-get install -qq -y --no-install-recommends \
		libfcgi-perl perlmagick; \
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get install -qq -y --no-install-recommends \
		git ca-certificates; \
        cd /; \
	git clone https://github.com/nabe-abk/adiary; \
	cd adiary; \
	git checkout $ADIARY_COMMIT; \
	cd ../; \
	rm -r adiary/.git; \
	apt-mark auto '.*' > /dev/null; \
	[ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; \
	apt-get purge -y --auto-remove \
		-o APT::AutoRemove::RecommendsImportant=false; \
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
