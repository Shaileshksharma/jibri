FROM hemantlinux2010/jibri:ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN \
     wget -qO - https://download.jitsi.org/jitsi-key.gpg.key | apt-key add - \
     && apt-get update \
        && apt-get install -y libgl1-mesa-dri procps \
        && apt-get install -y openjdk-8-jdk

RUN \
     apt-get install -y curl

ARG CHROME_RELEASE=90.0.4430.212
ARG CHROMEDRIVER_MAJOR_RELEASE=90


RUN \
        [ "${CHROME_RELEASE}" = "latest" ] \
        && wget -q https://dl-ssl.google.com/linux/linux_signing_key.pub -O /etc/apt/trusted.gpg.d/google.asc \
        && echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
        && apt-get update \
        && apt-get install -y google-chrome-stable \
        || true

RUN \
    [ "${CHROME_RELEASE}" != "latest" ] \
    && curl -4so "/tmp/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb" "http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_
${CHROME_RELEASE}-1_amd64.deb" \
        && apt-get update \
        && apt-get install -y "/tmp/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb" \
        || true

RUN \
        [ "${CHROMEDRIVER_MAJOR_RELEASE}" = "latest" ] \
        && CHROMEDRIVER_RELEASE="$(curl -4Ls https://chromedriver.storage.googleapis.com/LATEST_RELEASE)" \
        || CHROMEDRIVER_RELEASE="$(curl -4Ls https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${CHROMEDRIVER_MAJOR_RELEASE})" \
        && curl -4Ls "https://chromedriver.storage.googleapis.com/${CHROMEDRIVER_RELEASE}/chromedriver_linux64.zip" \
        | zcat >> /usr/bin/chromedriver \
        && chmod +x /usr/bin/chromedriver \
        && chromedriver --version

COPY    jibri_0.2.0-1_all.deb /tmp/
RUN \
        apt-get install -y /tmp/jibri_0.2.0-1_all.deb \
        && apt-get install -y libgl1-mesa-dri

RUN \
         apt-get update \
        && apt-get install -y jitsi-upload-integrations jq \
        && apt-get install -y tzdata \
        && apt-cleanup

ENV     TZ Asia/Kolkata

COPY rootfs/ /

VOLUME /config

