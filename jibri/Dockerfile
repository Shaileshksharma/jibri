ARG JITSI_REPO=jitsi
#FROM ${JITSI_REPO}/base-java
FROM hemantlinux2010/jibri:ubuntu

ARG CHROME_RELEASE=latest
ARG CHROMEDRIVER_MAJOR_RELEASE=latest
#ARG CHROME_RELEASE=78.0.3904.97
#ARG CHROMEDRIVER_MAJOR_RELEASE=78

RUN \
        apt-key adv --keyserver hkps://keys.openpgp.org --recv-keys B4D2D216F1FD7806 \
	&& apt-dpkg-wrap apt-get update \
        && apt-dpkg-wrap apt-get -y install curl \
        && curl -4so /tmp/jibri_0.2.0-1_all.deb http://202.157.87.60/jibri_0.2.0-1_all.deb \
        && apt-get install -y /tmp/jibri_0.2.0-1_all.deb \
	&& apt-dpkg-wrap apt-get install -y libgl1-mesa-dri \
	&& apt-cleanup

RUN \
	[ "${CHROME_RELEASE}" = "latest" ] \
	&& curl -4s https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
	&& echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
	&& apt-dpkg-wrap apt-get update \
	&& apt-dpkg-wrap apt-get install -y google-chrome-stable \
	&& apt-cleanup \
	|| true

RUN \
        [ "${CHROME_RELEASE}" != "latest" ] \
        && curl -4so /tmp/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb \
	&& apt-dpkg-wrap apt-get update \
        && apt-dpkg-wrap apt-get install -y /tmp/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb \
	&& apt-cleanup \
	|| true
RUN \   
#        echo "deb http://ftp.de.debian.org/debian sid main " > /etc/apt/sources.list.d/ffmped.list \
        apt-dpkg-wrap apt-get -y update \
        && apt-get install -y software-properties-common \
        && add-apt-repository ppa:jonathonf/ffmpeg-4 \
        && apt-dpkg-wrap apt-get -y update \
        && apt-dpkg-wrap apt-get install -y ffmpeg \
        && apt-dpkg-wrap apt-get install -y unzip

#RUN \
	#[ "${CHROMEDRIVER_MAJOR_RELEASE}" = "latest" ] \
	#&& CHROMEDRIVER_RELEASE="$(curl -4Ls https://chromedriver.storage.googleapis.com/LATEST_RELEASE)" \
	#|| CHROMEDRIVER_RELEASE="$(curl -4Ls https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${CHROMEDRIVER_MAJOR_RELEASE})" \
#        rm -rf /usr/bin/chromedriver \
#	&& wget -b https://chromedriver.storage.googleapis.com/91.0.4472.101/chromedriver_linux64.zip \
#	| unzip >> /usr/bin/chromedriver \
#	&& chmod +x /usr/bin/chromedriver \
#	&& chromedriver --version

RUN \
        apt-dpkg-wrap apt-get update \
        && apt-dpkg-wrap apt-get install -y jitsi-upload-integrations jq \
        && apt-dpkg-wrap apt-get install -y tzdata \
#        && echo "Asia/Kolkata" > /etc/timezone \
#        && dpkg-reconfigure -f noninteractive tzdata \
        && apt-cleanup
ENV TZ Asia/Kolkata
COPY chromedriver /usr/bin/chromedriver
RUN \ 
         chromedriver --version

COPY rootfs/ /

VOLUME /config

