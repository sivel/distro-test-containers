FROM centos:7.8.2003

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*; \
rm -f /etc/systemd/system/*.wants/*; \
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*; \
rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN yum clean all && \
    yum -y install epel-release && \
    yum -y install \
    acl \
    asciidoc \
    bzip2 \
    dbus-python \
    file \
    gcc \
    git \
    iproute \
    libffi \
    libffi-devel \
    make \
    mercurial \
    openssh-clients \
    openssh-server \
    openssl-devel \
    python-cryptography \
    python-devel \
    python-httplib2 \
    python-jinja2 \
    python-keyczar \
    python-lxml \
    python-mock \
    python-nose \
    python2-packaging \
    python2-passlib \
    python-pip \
    python-setuptools \
    python-virtualenv \
    PyYAML \
    rpm-build \
    rubygems \
    sshpass \
    subversion \
    sudo \
    unzip \
    which \
    && \
    yum clean all && \
    rm -rf /usr/share/fonts/* \
        /usr/share/i18n/* \
        /usr/share/sgml/docbook/xsl-stylesheets* \
        /usr/share/adobe/resources/*

RUN /usr/bin/sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers
RUN mkdir /etc/ansible/
RUN /usr/bin/echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts
VOLUME /sys/fs/cgroup /run /tmp
RUN ssh-keygen -q -t rsa1 -N '' -f /etc/ssh/ssh_host_key && \
    ssh-keygen -q -t dsa -N '' -f /etc/ssh/ssh_host_dsa_key && \
    ssh-keygen -q -t rsa -N '' -f /etc/ssh/ssh_host_rsa_key && \
    ssh-keygen -m PEM -q -t rsa -N '' -f /root/.ssh/id_rsa && \
    cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys && \
    for key in /etc/ssh/ssh_host_*_key.pub; do echo "localhost $(cat ${key})" >> /root/.ssh/known_hosts; done
RUN pip install 'coverage<5' junit-xml
ENV container=docker
CMD ["/usr/sbin/init"]
