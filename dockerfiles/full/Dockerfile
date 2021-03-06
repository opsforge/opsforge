# Dockerfile of opsforge.io DevUtils for PaaS use - Copyright (C) 2017 George Svachulay - Apache 2.0 License

FROM opsforge/anvil:lite

ENV VAULTVER="1.6.2"
ENV TFVER="0.14.6"
ENV BOSHVER="6.4.1"
ENV CONSULVER="1.9.3"

MAINTAINER opsforge.io
LABEL type="full"

# OS fixes and additions

USER root

# Ubuntu package installs

SHELL ["/bin/bash", "-c"]

RUN apt-get update && \
    curl -fsSL https://get.docker.com | bash && \
    apt-get clean && \
    mkdir -p /tmp

# PIP package installs

RUN pip3 install boto3

# Get docker compose

RUN curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose
    
# AWS CLI install

RUN mkdir -p ./awscli && \
    cd ./awscli && \
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    cd ../ && \
    rm -rf ./awscli

# Install KOPS

RUN cd /tmp && \
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64 && \
    chmod +x kops-linux-amd64 && \
    mv kops-linux-amd64 /usr/local/bin/kops

# Install KubeCTL

RUN cd /tmp && \
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && \
    chmod +x ./kubectl && \
    mv ./kubectl /usr/local/bin/kubectl

# Install VAULT

RUN cd /tmp && \
    curl -LO https://releases.hashicorp.com/vault/${VAULTVER}/vault_${VAULTVER}_linux_amd64.zip && \
    unzip vault_${VAULTVER}_linux_amd64.zip && \
    chmod +x vault && \
    mv vault /usr/local/bin/vault && \
    rm -rf /tmp/*

# Install terraform

RUN cd /tmp && \
    curl -LO https://releases.hashicorp.com/terraform/${TFVER}/terraform_${TFVER}_linux_amd64.zip && \
    unzip terraform_${TFVER}_linux_amd64.zip && \
    chmod +x terraform && \
    mv terraform /usr/local/bin/terraform && \
    rm -rf /tmp/*

# Install Consul
RUN cd /tmp && \
    wget "https://releases.hashicorp.com/consul/${CONSULVER}/consul_${CONSULVER}_linux_amd64.zip" && \
    unzip consul_* && \
    chmod +x consul && \
    mv consul /usr/local/bin/ && \
    rm -rf /tmp/*
    
# Install Certbot

RUN apt-get update && \
    # dep fix for pip3
    pip3 install urllib3 chardet requests --upgrade && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y certbot && \
    apt-get clean

# Install NodeJS

RUN apt-get update && \
    apt-get install nodejs npm -y && \
    apt-get clean

# Install Ruby stable

RUN cd /tmp && \
    curl -sSL https://rvm.io/mpapis.asc | gpg --import - && \
    curl -sSL https://rvm.io/pkuczynski.asc | gpg --import - && \
    curl -sSL https://get.rvm.io | bash -s stable --ruby && \
    rm -rf /tmp/*

# Install CF CLI using pivotal method

RUN cd /tmp && \
    curl -L -o cf.deb "https://cli.run.pivotal.io/stable?release=debian64&source=github" && \
    dpkg -i cf.deb && \
    rm -rf /tmp/*

# Install OM

RUN cd /tmp && \
    curl -LO https://github.com/pivotal-cf/om/releases/download/$(curl -s https://github.com/pivotal-cf/om/releases/latest | sed 's/^.*tag\///' | sed 's/\".*$//')/om-linux && \
    chmod +x om-linux && \
    mv om-linux /usr/local/bin/om

# Install BOSH cli

RUN cd /tmp && \
    curl -LO https://s3.amazonaws.com/bosh-cli-artifacts/bosh-cli-${BOSHVER}-linux-amd64 && \
    chmod +x bosh-cli-${BOSHVER}-linux-amd64 && \
    mv bosh-cli-${BOSHVER}-linux-amd64 /usr/local/bin/bosh

# Install FLY CLI

RUN cd /tmp && \
    curl -LO https://github.com/concourse/concourse/releases/download/$(curl -s https://github.com/concourse/concourse/releases/latest | sed 's/^.*tag\///' | sed 's/\".*$//')/fly_linux_amd64 && \
    chmod +x fly_linux_amd64 && \
    mv fly_linux_amd64 /usr/local/bin/fly

RUN rm -rf /tmp/*

SHELL ["/bin/zsh", "-c"]
CMD ["/root/entrypoint.sh"]
