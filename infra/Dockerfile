FROM ubuntu:22.04

# Install required packages
RUN apt-get update && apt-get install -y openssh-server sudo python3

# Configure SSH and set a temporary root password
RUN mkdir /var/run/sshd
RUN echo 'root:root' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Expose standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
