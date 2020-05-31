FROM debian:10
# if we want to install via apt
USER root

RUN apt-get --fix-missing update && \
    apt-get -y install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
        software-properties-common
RUN curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add - >>
/dev/null 2>&1 && \
    curl -fsSL https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - >> /dev/null 2>&1 && \
    add-apt-repository \
        "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" && \
    add-apt-repository \
      "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/$(. /etc/os-release; echo "$ID")  $(lsb_release -cs) stable" && \
    apt-get update --fix-missing && \
    apt-get -y install docker-ce-cli kubectl gnupg2 pass && \
    rm -rf /var/lib/apt/lists/*

COPY image-files/ /
ENV DOCKER_HOST=tcp://192.168.163.91:2375 \
    DOCKER_TLS_VERIFY=1 \
