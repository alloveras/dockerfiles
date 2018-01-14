FROM centos:latest

# Define SSH_USER environment variable with its default value
# set to 'admin'.
ARG SSH_USER="admin"

# Update yum repositories.
RUN yum update -y

# Install ssh server and sudo command.
RUN yum install -y openssh-server sudo

# Enable epel repository and install ansible.
RUN yum install -y epel-release && \
    yum install -y ansible

# Create RSA key pairs for new container.
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''

# Create the new ssh user and grant it with sudo privileges.
RUN adduser "${SSH_USER}"
RUN passwd -l "${SSH_USER}"
RUN echo "${SSH_USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Populate user's home directory with SSH authorized public keys.
RUN mkdir -p "/home/${SSH_USER}/.ssh"
ADD id_rsa "/home/${SSH_USER}/.ssh/id_rsa"
ADD id_rsa.pub "/home/${SSH_USER}/.ssh/id_rsa.pub"
ADD id_rsa.pub "/home/${SSH_USER}/.ssh/authorized_keys"
RUN chown -R "${SSH_USER}":"${SSH_USER}" "/home/${SSH_USER}/.ssh"
RUN chmod 700 "/home/${SSH_USER}/.ssh"

# Swap default SSH config file with a security tweaked one.
COPY sshd_config /etc/ssh/sshd_config

EXPOSE 22
CMD    ["/usr/sbin/sshd", "-D"]