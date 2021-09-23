FROM perl:5-buster

MAINTAINER yanorei32 <yanorei@hotmail.co.jp>
 
RUN set -ex; \
	apt-get update -qq; \
	apt-get install -qq -y --no-install-recommends \
		perlmagick libnet-ssleay-perl libcryptx-perl libfcgi-perl; \
	wget -q -O adiary.tar.gz https://github.com/nabe-abk/adiary/archive/master.tar.gz; \
	tar xf adiary.tar.gz; \
	rm adiary.tar.gz \
	rm -rf /var/lib/apt/lists/*; \
	mkdir -p /adiary-master/shared/; \
	mv /adiary-master/pub/ /adiary-master/shared/; \
	mv /adiary-master/pub-dist/ /adiary-master/shared/; \
	mv /adiary-master/theme/ /adiary-master/shared/; \
	mv /adiary-master/js/ /adiary-master/shared/; \
	mkdir -p /adiary-master/private/; \
	mv /adiary-master/data/ /adiary-master/private/; \
	mv /adiary-master/plugin/ /adiary-master/private/; \
	mv /adiary-master/skel.local/ /adiary-master/private/; \
	cat /adiary-master/adiary.conf.cgi.sample \
		| sed 's:pub/:shared/pub/:' \
		| sed 's:pub-dist/:shared/pub-dist/:' \
		| sed 's:theme/:shared/theme/:' \
		| sed 's:js/:shared/js/:' \
		| sed 's:data/:private/data/:' \
		| sed 's:plugin/:private/plugin/:' \
		| sed 's:skel.local/:private/skel.local/:' \
		> /adiary-master/adiary.conf.cgi;

VOLUME /adiary-master/shared/ /adiary-master/private/

EXPOSE 9000
CMD /adiary-master/adiary.fcgi 0.0.0.0:9000
