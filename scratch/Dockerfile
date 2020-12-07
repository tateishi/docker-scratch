FROM ubuntu:20.04

ARG PROXY
ENV HTTP_PROXY=${PROXY}
ENV HTTPS_PROXY=${PROXY}

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y \
        language-pack-ja-base \
        language-pack-ja \
        fonts-noto-cjk \
        fcitx-mozc dbus-x11 \
    && im-config -n fcitx


RUN apt-get update && apt-get install -y \
        git \
        build-essential \
        bash-completion \
        x11-apps \
        terminator \
        scratch \
        curl

RUN apt-get update && apt-get install -y \
        cmake \
        g++ \
        xorg-dev


RUN sed -e 's/\(VMOPTIONS="-encoding UTF-8[^"]*\)"/\1 -compositioninput"/' -i.bak /usr/bin/scratch

ENV GTK_IM_MODULE=fcitx \
    QT_IM_MODULE=fcitx \
    XMODIFIERS=@im=fcitx \
    DefalutIMModule=fcitx

RUN locale-gen ja_JP.UTF-8
ENV LANG=ja_JP.UTF-8 \
    LC_ALL=ja_JP.UTF-8

RUN groupadd -g 1000 ubuntu \
        && useradd -u 1000 -g ubuntu \
        -m -s /bin/bash ubuntu

USER ubuntu
WORKDIR /home/ubuntu

RUN echo "fcitx-autostart" >> .bashrc

RUN mkdir -p /home/ubuntu/wks
WORKDIR /home/ubuntu/wks

# fetch squeak tarball and patch
RUN curl http://ftp.de.debian.org/debian/pool/main/s/squeak-vm/squeak-vm_4.10.2.2614.orig.tar.gz -O
RUN tar xzf squeak-vm_4.10.2.2614.orig.tar.gz
COPY ./utf.patch .
RUN patch -u -p1 < utf.patch

# build module
RUN mkdir /home/ubuntu/wks/Squeak-4.10.2.2614-src/build
WORKDIR /home/ubuntu/wks/Squeak-4.10.2.2614-src/build
RUN ../unix/cmake/configure
RUN make vm-display-X11

# install module
USER root
RUN cp vm-display-X11/so.vm-display-X11 /usr/lib/squeak/4.10.2.2614/

USER ubuntu
WORKDIR /home/ubuntu
