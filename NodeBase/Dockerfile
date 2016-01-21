FROM selenium/base:python2.7
MAINTAINER Selenium <selenium-developers@googlegroups.com>

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

#===================
# Timezone settings
# Possible alternative: https://github.com/docker/docker/issues/3359#issuecomment-32150214
#===================
ENV TZ "US/Pacific"
RUN echo "US/Pacific" | sudo tee /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata

#==============
# VNC and Xvfb
#==============
RUN apt-get update -qqy \
  && apt-get -qqy install \
    xvfb \
    build-essential checkinstall \ # python2.7 build deps
    libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev \
  && rm -rf /var/lib/apt/lists/*
#==============
# python2.7.10 src download
#==============

RUN cd /usr/src \
  && wget https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tgz \
  && tar xzf Python-2.7.10.tgz

# python2.7.10 compile

RUN cd Python-2.7.10 \
  && ./configure \
  && make altinstall

# python2.7 install selenium

RUN python2.7 -m ensurepip
RUN python2.7 -m pip install selenium

#==============================
# Scripts to run Selenium Node
#==============================
COPY entry_point.sh /opt/bin/entry_point.sh
RUN chmod +x /opt/bin/entry_point.sh

#============================
# Some configuration options
#============================
ENV SCREEN_WIDTH 1360
ENV SCREEN_HEIGHT 1020
ENV SCREEN_DEPTH 24
ENV DISPLAY :99.0

USER seluser

CMD ["/opt/bin/entry_point.sh"]
