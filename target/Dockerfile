# Lubuntu Linux with ansible target

FROM youske/ubuntu-stdmachine:base
MAINTAINER youske miyakoshi <youske@gmail.com>

LABEL com.stdmachine.description="ubuntu ansible target image" \
      com.stdmachine.role="ansible target" \
      com.stdmachine.os="ubuntu"

# apt-get install
RUN COMMONPACKAGE='openssh-server openssh-client git vim' && \
    DEBIAN_FRONTEND=nointeractive apt-get update && apt-get -y upgrade && \
    DEBIAN_FRONTEND=nointeractive apt-get install -y ${COMMONPACKAGE} ${LANGPACKAGE} && \
    # ssh access
    SSHKEY_URL=https://gist.githubusercontent.com/youske/e92f8c00c9b9a38a20de383982362d22/raw/3a6d71d5464bf0baf38fc990a87d8632695d58ef/sk_testkey && \
    SSHPUB_URL=https://gist.githubusercontent.com/youske/e92f8c00c9b9a38a20de383982362d22/raw/5d71802d99132a66a24220818d5baa217eb18e31/sk_testkey.pub && \
    cd /home/admin && mkdir -p .ssh && cd .ssh && \
    wget -q ${SSHKEY_URL} -O sshkey_public && chmod go-r sshkey_public && \
    wget -q ${SSHPUB_URL} -O authorized_keys && \
    chown -R admin:admin /home/admin/.ssh && \
    # remove build package
    apt-get autoremove && rm -rf /tmp/* /var/lib/apt/lists/*

COPY sshd_config /etc/ssh/

EXPOSE 22
ENTRYPOINT ["switch","root=/bin/bash","--","switch","shell=gosu admin /bin/bash","--","/init"]
CMD ["gosu","admin","/bin/bash"]
