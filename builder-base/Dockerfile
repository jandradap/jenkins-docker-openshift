FROM openshift/origin-cli:v3.11

LABEL maintainer="Arnaud Deprez <arnaudeprez@gmail.com>"

ENV HOME=/home/jenkins
WORKDIR /tmp

RUN yum install -y epel-release && \
    INSTALL_PKGS="bc gettext git lsof make rsync tar unzip wget which zip bzip2 jq" && \
    yum install -y $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all && \
    curl -sSL https://github.com/mikefarah/yq/releases/download/2.3.0/yq_linux_amd64 -o /usr/local/bin/yq && \
    chmod -R 755 /usr/local/bin && \
    mkdir -p /home/jenkins && \
    chown -R 1001:0 /home/jenkins && \
    chmod -R g+w /home/jenkins && \
    chmod 664 /etc/passwd && \
    chmod 775 /usr/bin && \
    chmod 775 /usr/share/man/man1 && \
    chmod 775 /var/lib/origin

ADD bin/ /usr/local/bin/
ADD test $HOME/test

RUN chmod -R 755 /usr/local/bin $HOME/test

# helm
ENV HELM_VERSION=2.13.1 \
    HELM_HOME=$HOME/.helm
RUN curl https://storage.googleapis.com/kubernetes-helm/helm-v${HELM_VERSION}-linux-amd64.tar.gz  | tar xzv && \
  mv linux-amd64/helm /usr/bin/ && \
  chown -R 1001:0 /usr/bin && \
  chmod -R g+w /usr/bin && \
  helm init --client-only && \
  helm plugin install https://github.com/chartmuseum/helm-push && \
  chown -R 1001:0 /home/jenkins && \
  chmod -R g+w /home/jenkins && \
  rm -rf linux-amd64

USER 1001

ENTRYPOINT [ "/usr/local/bin/run" ]