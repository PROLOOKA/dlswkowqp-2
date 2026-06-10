FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    openssh-server && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /var/run/sshd

RUN echo "root:lookmora" | chpasswd

RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config && \
    echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config && \
    echo "UseDNS no" >> /etc/ssh/sshd_config && \
    echo "ClientAliveInterval 60" >> /etc/ssh/sshd_config && \
    echo "ClientAliveCountMax 3" >> /etc/ssh/sshd_config

EXPOSE 22

CMD ["/usr/sbin/sshd","-D","-e"]
