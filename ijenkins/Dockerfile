FROM jenkins/jenkins:lts
# if we want to install via apt
USER root

RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
    sed -i \
        -e 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/g' \
        -e 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/g' \
        /etc/apt/sources.list && \
    apt-get update && \
    apt-get -y install \
        maven \
        nodejs \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
        software-properties-common
RUN curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add - && \
    add-apt-repository \
      "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/$(. /etc/os-release; echo "$ID")  $(lsb_release -cs) stable" && \
    apt-get update --fix-missing && \
    apt-get -y install docker-ce && \
    rm -rf /var/lib/apt/lists/* && \
    sed -i -e '$ a export DOCKER_HOST=tcp://192.168.0.2:32375 DOCKER_TLS_VERIFY=1' /etc/profile && \
    export DOCKER_HOST=tcp://192.168.0.2:32375 DOCKER_TLS_VERIFY=1
COPY image-files/ /
#    sed -i -e '$ a export DOCKER_HOST=tcp://171.213.19.251:32375' /etc/profile && \
#    export DOCKER_HOST=tcp://171.213.19.251:32375