# Dockerfile
FROM ubuntu:22.04
RUN apt-get update && apt-get install -y openssh-server python3 sudo
RUN mkdir /var/run/sshd
RUN useradd -m ansible && echo "ansible:ansible" | chpasswd && adduser ansible sudo
RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
