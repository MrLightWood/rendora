# based on justinribeiro/chrome-headless but modified to suit Rendora's goal for server-side rendering
# current default Chrome flags are recommended to be used for your own website purposes only
# based on rendora/dockerfiles: https://github.com/rendora/dockerfiles
FROM ubuntu:noble
ENV DEBIAN_FRONTEND=noninteractive

ARG CHROME_VERSION="135.0.7049.84-1"
LABEL name="chrome-headless" \
			version="1.0" \
			description="Headless Chrome"

RUN apt-get update && apt-get install -y \
        curl \
	apt-transport-https \
	ca-certificates \
 	fontconfig \
	wget \
	gnupg \
	--no-install-recommends \
        && wget --no-verbose -O /tmp/chrome.deb https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${CHROME_VERSION}_amd64.deb \
	&& apt install -y /tmp/chrome.deb \
        && rm -rf /var/lib/apt/lists/* /tmp/chrome.deb

RUN groupadd -r chrome && useradd -r -g chrome -G audio,video chrome \
    && mkdir -p /home/chrome && chown -R chrome:chrome /home/chrome \
		&& mkdir -p /opt/google/chrome && chown -R chrome:chrome /opt/google/chrome

ENV TINI_VERSION=v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
USER chrome

EXPOSE 9222

ENTRYPOINT ["/tini", "--", "google-chrome"]
CMD ["--headless", "--no-sandbox", "--disable-setuid-sandbox", \
                "--disable-dev-shm-usage", "--disable-gpu", "--remote-debugging-address=0.0.0.0", \
                "--remote-debugging-port=9222" , "--disable-remote-fonts", "--user-data-dir=/tmp"]
