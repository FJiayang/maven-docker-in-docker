FROM fjy8018/maven-docker-in-docker:maven-3.6.3-docker-1903-jdk8


COPY dockerd-entrypoint.sh /usr/local/bin/

RUN set -eux; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
		btrfs-progs \
		e2fsprogs \
		xfsprogs \
		xz-utils \
		pigz \
		iptables \
	; \
	rm -rf /var/lib/apt/lists/*

# TODO aufs-tools

# set up subuid/subgid so that "--userns-remap=default" works out-of-the-box
RUN set -x \
	&& adduser dockremap \
	&& echo 'dockremap:165536:65536' >> /etc/subuid \
	&& echo 'dockremap:165536:65536' >> /etc/subgid

# https://github.com/docker/docker/tree/master/hack/dind
ENV DIND_COMMIT 37498f009d8bf25fbb6199e8ccd34bed84f2874b

RUN set -eux; \
	wget -O /usr/local/bin/dind "https://raw.githubusercontent.com/docker/docker/${DIND_COMMIT}/hack/dind"; \
	chmod +x /usr/local/bin/dind

VOLUME /var/lib/docker
EXPOSE 2375

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
CMD []
