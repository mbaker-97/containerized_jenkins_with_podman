FROM docker.io/redhat/ubi8:8.5

#Install podman 
RUN dnf -y update; yum -y reinstall shadow-utils; \ 
yum -y install podman fuse-overlayfs --exclude container-selinux; \
rm -rf /var/cache /var/log/dnf* /var/log/yum.*; \
yum -y install wget git ;\
yum -y install gcc python2 # kernel-devel kernel-headers

RUN useradd podman; \
echo podman:10000:5000 > /etc/subuid; \
echo podman:10000:5000 > /etc/subgid;

RUN mkdir /jenkins

#For rootfull containers
VOLUME /var/lib/containers
#For rootless containers
VOLUME /var/podman/.local/containers

ADD https://raw.githubusercontent.com/containers/libpod/master/contrib/podmanimage/stable/containers.conf /etc/containers/containers.conf

ADD https://raw.githubusercontent.com/containers/libpod/master/contrib/podmanimage/stable/podman-containers.conf /home/podman/.config/containers/containers.conf

RUN wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo; \
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key; \
dnf upgrade; \

# Add required dependencies for the jenkins package 
dnf -y install java-11-openjdk; \
dnf -y install jenkins;
ENV JENKINS_HOME=/jenkins
CMD  ["jenkins"]
