# Centos 7 (ansible)

Dockerfile that builds up a CentOS7 container image with the following software stack:

 - OpenSSH Server
 - Ansible

> **Note**: This image contains a password-less sudo user. This means that you won't need to configure any password neither to access the machine through SSH nor to execute `sudo` commands.

 ## Build your docker image

1. [Optional] Create a new SSH RSA key-pair if you don't have one:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

> **Note**: The key-pair generation tool will prompt you some questions during the process. Unless you know what you are doing, press ENTER to use the default values.

1. Copy your SSH RSA public key to the same directory where `Dockerfile` is located:
```
cp ~/.ssh/id_rsa.pub ./id_rsa.pub
```

1. Copy your SSH RSA private key to the same directory where `Dockerfile` is located:
```
cp ~/.ssh/id_rsa ./id_rsa
```

1. Build up a docker image customized for your user:
 ```
 docker build -t $(whoami)/centos7-ansible . --build-arg SSH_USER=$(whoami)
 ```
> **Note**: To build a docker image for any other user rather than the one that executes the command replace `$(whoami)` for the desired username.

1. Create a new container using the new image:
```
docker run -d --name=<container_name> -p <host_port>:22 $(whoami)/centos7-ansible
```

1. Connect to your new container using SSH:
```
ssh -p <host_port> localhost
```
