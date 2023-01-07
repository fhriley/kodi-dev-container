ARG BASE_IMAGE="ubuntu:22.04"
ARG EASY_NOVNC_IMAGE="fhriley/easy-novnc:1.3.0"

FROM $EASY_NOVNC_IMAGE as easy-novnc
FROM $BASE_IMAGE

COPY --from=easy-novnc /usr/local/bin/easy-novnc /usr/local/bin/easy-novnc

ARG DEBIAN_FRONTEND="noninteractive"

RUN apt-get update -y \
  && apt install -y software-properties-common

RUN yes | unminimize

RUN add-apt-repository -y -s ppa:team-xbmc/xbmc-nightly \
  && apt build-dep -y kodi

RUN apt-get install -y \
    alsa-base \
    ca-certificates \
    ccache \
    clang-format \
    curl \
    gdb \
    git \
    gosu \
    libavcodec-dev \
    libavfilter-dev \
    libavformat-dev \
    libavutil-dev \
    libkissfft-dev \
    libswresample-dev \
    libswscale-dev \
    openssh-server \
    rsync \
    sudo \
    supervisor \
    tigervnc-standalone-server \
    tzdata

RUN echo 'pcm.!default = null;' > /etc/asound.conf
COPY supervisord.conf /etc/

RUN mkdir /var/run/sshd \
  && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
  && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN useradd --home-dir /home/vscode --create-home --shell /bin/bash --groups sudo vscode

RUN echo "vscode:vscode" | chpasswd

CMD ["supervisord", "-c", "/etc/supervisord.conf"]
