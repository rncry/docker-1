FROM centos

MAINTAINER Humble Chirammal hchiramm@redhat.com

ENV container docker

RUN yum --setopt=tsflags=nodocs -y update

RUN yum --setopt=tsflags=nodocs -y install wget

RUN yum --setopt=tsflags=nodocs -y install nfs-utils

RUN yum --setopt=tsflags=nodocs -y install openssh-server

RUN yum -y swap -- remove fakesystemd -- install systemd systemd-libs

RUN yum -y update; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN wget http://download.gluster.org/pub/gluster/glusterfs/3.7/LATEST/CentOS/glusterfs-epel.repo -O /etc/yum.repos.d/glusterfs-epel.repo

RUN yum -y update

RUN yum --setopt=tsflags=nodocs -y install glusterfs glusterfs-server glusterfs-fuse glusterfs-geo-replication glusterfs-cli glusterfs-api
RUN yum --setopt=tsflags=nodocs -y install attr
RUN yum --setopt=tsflags=nodocs -y install iputils iproute
RUN yum clean all

RUN echo 'root:password' | chpasswd
VOLUME [ “/sys/fs/cgroup” ]

EXPOSE 22 111 245 443 24007 2049 8080 6010 6011 6012 38465 38466 38468 38469 49152 49153 49154 49156 49157 49158 49159 49160 49161 49162

RUN systemctl disable nfs-server.service
RUN systemctl enable rpcbind.service
RUN systemctl enable sshd.service
RUN systemctl enable glusterd.service
CMD ["/usr/sbin/init"]
