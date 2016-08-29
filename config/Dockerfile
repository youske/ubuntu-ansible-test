# Lubuntu Linux with ansible master

FROM youske/ubuntu-stdmachine:base
MAINTAINER youske miyakoshi <youske@gmail.com>

LABEL com.stdmachine.description="ubuntu standard image" \
      com.stdmachine.role="base image" \
      com.stdmachine.os="ubuntu"

#----- common -----

# s6-overlay
ENV S6OVERLAY_VERSION="v1.11.0.1"
ADD "https://github.com/just-containers/s6-overlay/releases/download/${S6OVERLAY_VERSION}/s6-overlay-amd64.tar.gz" /tmp/

# entrykit
ENV ENTRYKIT_VERSION=0.4.0
ENV ENTRYKIT_FILE=entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz
ADD "https://github.com/progrium/entrykit/releases/download/v${ENTRYKIT_VERSION}/${ENTRYKIT_FILE}" /tmp/

ENV GOSU_VERSION=1.7
ADD "https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64" /tmp/

# apt-get install
RUN COMMONPACKAGE='bash git bzip2 sudo wget ca-certificates ' && \
    LANGPACKAGE='locales language-pack-ja ' && \
    DEBIAN_FRONTEND=nointeractive apt-get update && apt-get -y upgrade && \
    DEBIAN_FRONTEND=nointeractive apt-get install -y ${COMMONPACKAGE} ${LANGPACKAGE} && \
    update-locale LANG=${LANGUAGE:-ja_JP.UTF-8} && \
    # entrykit install
    tar zxvf /tmp/${ENTRYKIT_FILE} -C /usr/bin && chmod +x /usr/bin && /usr/bin/entrykit --symlink && \
    # gosu
    mv /tmp/gosu-amd64 /usr/bin/gosu && chmod +x /usr/bin/gosu && \
    # s6-overlay
    tar zxf /tmp/s6-overlay-amd64.tar.gz -C / && \
    # create admin user
    useradd -m -U -s /bin/bash admin && echo "admin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    # remove build package
    apt-get autoremove && rm -rf /tmp/*

ENTRYPOINT ["switch","root=/bin/bash","--","switch","shell=gosu admin /bin/bash","--","/init"]
CMD ["/bin/bash"]
