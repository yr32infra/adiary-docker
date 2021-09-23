FROM alpine

# I found cloudflare/cloudflared.
#   But, cloudflare/cloudflared is not compatible with arm64.
#
# https://hub.docker.com/r/cloudflare/cloudflared
MAINTAINER yanorei32 <yanorei@hotmail.co.jp>

ADD \
	https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm64 \
	/cloudflared

RUN chmod +x /cloudflared

ENTRYPOINT [ "/cloudflared" ]
CMD [ "version" ]
