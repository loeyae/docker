FROM bitnami/jenkins:2.222.4
# if we want to install via apt
USER root

#RUN grep _apt /etc/passwd /etc/shadow && \
#    cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
#    echo "deb http://mirrors.aliyun.com/debian/ buster main non-free contrib" >/etc/apt/sources.list && \
#    echo "deb http://mirrors.aliyun.com/debian-security buster/updates main" >>/etc/apt/sources.list && \
#    echo "deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib" >>/etc/apt/sources.list && \
#    echo "deb http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib" >>/etc/apt/sources.list && \
#    echo "deb-src http://mirrors.aliyun.com/debian-security buster/updates main" >>/etc/apt/sources.list && \
#    echo "deb-src http://mirrors.aliyun.com/debian/ buster main non-free contrib" >>/etc/apt/sources.list && \
#    echo "deb-src http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib" >>/etc/apt/sources.list && \
#    echo "deb-src http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib" >>/etc/apt/sources.list && \
RUN apt-get update && \
    apt -y install dpkg-dev && \
    apt-get -y install \
        maven \
        nodejs \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
        software-properties-common
RUN curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add - && \
    curl -fsSL https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - && \
    add-apt-repository \
        "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" && \
    add-apt-repository \
      "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/$(. /etc/os-release; echo "$ID")  $(lsb_release -cs) stable" && \
    apt-get update --fix-missing && \
    apt-get -y install docker-ce-cli kubectl gnupg2 pass maven && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /opt/bitnami/jenkins/repo && \rm -rf /usr/bin/java

COPY image-files/ /
ENV JAVA_HOME=/opt/bitnami/j2sdk \
    DOCKER_HOST=tcp://119.4.240.179:2375 \
    DOCKER_TLS_VERIFY=1 \
    PATH=$PATH:/opt/bitnami/git/bin:/opt/bitnami/nami/bin:$JAVA_HOME/bin
