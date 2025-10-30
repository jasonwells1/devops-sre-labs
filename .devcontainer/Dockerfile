# syntax=docker/dockerfile:1.7-labs
FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ARG TERRAFORM_VERSION=1.9.6
ARG KUBECTL_VERSION=1.30.3
ARG HELM_VERSION=3.15.3
ARG AWSCLI_VERSION=2.17.64
ARG TFSEC_VERSION=1.28.6
ARG TFLINT_VERSION=0.53.0
ARG TRIVY_VERSION=0.55.2

USER root
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    curl ca-certificates unzip gzip tar jq gnupg lsb-release apt-transport-https \
    python3 python3-pip make git openssh-client wget xz-utils && \
    rm -rf /var/lib/apt/lists/*

# awscli v2
RUN curl -fsSL "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-.zip" -o awscliv2.zip \
  && unzip -q awscliv2.zip \
  && ./aws/install \
  && rm -rf aws awscliv2.zip

# terraform
RUN curl -fsSLO https://releases.hashicorp.com/terraform//terraform__linux_amd64.zip \
  && unzip -q terraform__linux_amd64.zip -d /usr/local/bin \
  && rm terraform__linux_amd64.zip

# kubectl
RUN curl -fsSLo /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/v/bin/linux/amd64/kubectl \
  && chmod +x /usr/local/bin/kubectl

# helm
RUN curl -fsSL https://get.helm.sh/helm-v-linux-amd64.tar.gz | tar xz \
  && mv linux-amd64/helm /usr/local/bin/helm \
  && rm -rf linux-amd64

# tfsec
RUN curl -fsSLo /usr/local/bin/tfsec https://github.com/aquasecurity/tfsec/releases/download/v/tfsec-linux-amd64 \
  && chmod +x /usr/local/bin/tfsec

# tflint
RUN curl -fsSLo /tmp/tflint.zip https://github.com/terraform-linters/tflint/releases/download/v/tflint_linux_amd64.zip \
  && unzip -q /tmp/tflint.zip -d /usr/local/bin \
  && rm /tmp/tflint.zip

# trivy
RUN curl -fsSLo /tmp/trivy.tar.gz https://github.com/aquasecurity/trivy/releases/download/v/trivy__Linux-64bit.tar.gz \
  && tar -xzf /tmp/trivy.tar.gz -C /usr/local/bin trivy \
  && rm /tmp/trivy.tar.gz

# yq (mikefarah)
RUN curl -fsSLo /usr/local/bin/yq https://github.com/mikefarah/yq/releases/download/v4.44.3/yq_linux_amd64 \
  && chmod +x /usr/local/bin/yq

# pre-commit
RUN pip3 install --no-cache-dir pre-commit==3.8.0

# non-root user is vscode
USER vscode
WORKDIR /workspaces
