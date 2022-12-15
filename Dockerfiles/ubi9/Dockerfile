FROM registry.access.redhat.com/ubi9:9.1.0
MAINTAINER sig-platform@spinnaker.io


ENV KUBECTL_RELEASE=1.22.0
ENV AWS_BINARY_RELEASE_DATE=2020-02-22
ENV AWS_CLI_VERSION=1.18.18
RUN yum -y install java-11-openjdk-headless.x86_64 wget vim   python3-pip net-tools jq  && \
pip3 install awscli==1.18.18
   
RUN yum -y update
RUN echo '#!/usr/bin/env bash' | tee /usr/local/bin/hal > /dev/null && \
  echo '/opt/halyard/bin/hal "$@"' | tee /usr/local/bin/hal > /dev/null
RUN chmod +x /usr/local/bin/hal

RUN curl -f -LO --retry 3 --retry-delay 3 https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_RELEASE}/bin/linux/amd64/kubectl && \
    chmod +x ./kubectl && \
    mv ./kubectl /usr/local/bin/kubectl

RUN curl -f -o  /usr/local/bin/aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/${KUBECTL_RELEASE}/${AWS_BINARY_RELEASE_DATE}/bin/linux/amd64/aws-iam-authenticator && \
    chmod +x /usr/local/bin/aws-iam-authenticator

RUN adduser spinnaker
COPY halyard-web/build/install/halyard /opt/halyard
USER spinnaker

CMD ["/opt/halyard/bin/halyard"]
