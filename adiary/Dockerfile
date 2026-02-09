FROM perl:5.43.7-slim-bookworm

# depName=git@github.com:nabe-abk/adiary.git
ARG ADIARY_COMMIT="b393f79b7642240bb827bfa9e496f0b86f86bca1"

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
