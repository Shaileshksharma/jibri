ARG JITSI_REPO=jitsi
#FROM ${JITSI_REPO}/base
FROM base_ubuntu

RUN	\
	mkdir -p /usr/share/man/man1 && \
	apt-dpkg-wrap apt-get update && \
#        echo "deb [trusted=yes] http://ftp.nl.debian.org/debian stretch main" > /etc/apt/sources.list.d/java.list && \
        apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y openjdk-8-jre && \
	apt-cleanup

