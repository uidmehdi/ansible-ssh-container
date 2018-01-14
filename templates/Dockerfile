# Latest version of ubuntu
FROM ubuntu:16.04

# If you're behind a proxy!
#ENV http_proxy 'http://192.168.110.1:8123'
#ENV https_proxy 'http://192.168.110.1:8123'

# Update and Install SSH
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd

# If you'd like to permit root user login through ssh
#RUN echo 'root:screencast' | chpasswd
#RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# Configure SSH and a sudoer user
# Note that "-G sudo" enables running in "sudo" group, that is, in Ubuntu, the sudoers group
RUN useradd --create-home -G sudo -s/bin/bash {{user_name}} -u {{user_uid}} -p $(openssl passwd -1 '{{upassword}}')

# Generate user's key pair
RUN mkdir /home/{{user_name}}/.ssh/
RUN ssh-keygen -q -N "" -t rsa -f /home/{{user_name}}/.ssh/ssh_host_rsa_key

ADD id_rsa.pub  /home/{{user_name}}/.ssh/authorized_keys

# Set the appropriate rights to the files
RUN chown {{user_name}}:{{user_name}} /home/{{user_name}}/.ssh && chown {{user_name}}:{{user_name}} /home/{{user_name}}/.ssh/*
RUN chmod 700 /home/{{user_name}}/.ssh && chmod 600 /home/{{user_name}}/.ssh/*

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
