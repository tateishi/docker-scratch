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
        scratch

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
