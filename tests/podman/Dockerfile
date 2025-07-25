FROM fedora:latest

ENV GOPATH=/root/go
ENV PATH=/usr/bin:/usr/sbin:/root/go/bin:/usr/local/bin::/usr/local/sbin

RUN dnf install -y awk golang python git gcc automake autoconf libcap-devel \
    systemd-devel yajl-devel libseccomp-devel go-md2man catatonit \
    glibc-static python3-libmount libtool make podman xz nmap-ncat procps-ng slirp4netns \
    device-mapper-devel containernetworking-plugins 'dnf-command(builddep)' && \
    dnf builddep -y podman && \
    chmod 755 /root && \
    git clone --depth=1 https://github.com/containers/podman /root/go/src/github.com/containers/podman && \
    cd /root/go/src/github.com/containers/podman && \
    make .install.ginkgo && cp ./bin/ginkgo /usr/local/bin && \
    make

## Change default log driver to k8s-file for tests
RUN sed -i 's/journald/k8s-file/g' /usr/share/containers/containers.conf
RUN echo containers:200000:268435456 >> /etc/subuid && echo containers:200000:268435456 >> /etc/subgid
COPY run-tests.sh /usr/local/bin
WORKDIR /root/go/src/github.com/containers/podman
ENTRYPOINT /usr/local/bin/run-tests.sh
