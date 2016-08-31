# Lubuntu Linux with ansible target

FROM youske/ubuntu-stdmachine:base
MAINTAINER youske miyakoshi <youske@gmail.com>

LABEL com.stdmachine.description="ubuntu ansible target image" \
      com.stdmachine.role="ansible target" \
      com.stdmachine.os="ubuntu"

# apt-get install
RUN COMMONPACKAGE='openssh-server openssh-client supervisor git vim' && \
    DEBIAN_FRONTEND=nointeractive apt-get update && apt-get -y -q upgrade && \
    DEBIAN_FRONTEND=nointeractive apt-get install -y -q ${COMMONPACKAGE} ${LANGPACKAGE} && \
    # ssh access
    SSHKEY_URL="https://gist.githubusercontent.com/youske/e92f8c00c9b9a38a20de383982362d22/raw/8f6173698cafd00321655855727df7303b1eb92f/sk_dsa_testkey" && \
    SSHPUB_URL="https://gist.githubusercontent.com/youske/e92f8c00c9b9a38a20de383982362d22/raw/8a77ee97fbd41aa07c05da44efc7091be6fa4d89/sk_dsa_testkey.pub" && \
    cd /home/admin && mkdir -p .ssh && cd .ssh && \
    wget -q ${SSHKEY_URL} -O id_rsa && chmod go-r id_rsa && \
    wget -q ${SSHPUB_URL} -O authorized_keys && \
    chown -R admin:admin /home/admin/.ssh && \
    # remove build package
    apt-get autoremove && rm -rf /tmp/* /var/lib/apt/lists/*

COPY sshd_config /etc/ssh/
COPY ssh.conf /etc/supervisor/conf.d/

EXPOSE 22
ENTRYPOINT ["switch","root=/bin/bash","--","switch","shell=gosu admin /bin/bash","--","/init"]
# CMD ["/usr/bin/supervisord","-n"]
CMD ["/bin/bash"]
