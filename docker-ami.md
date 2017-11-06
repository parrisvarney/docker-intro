# Creating a Docker AMI

[Back to table of contents...](README.md)

## Choose the base AMI

A good base AMI is the [official CentOS 7](https://aws.amazon.com/marketplace/pp/B00O7WM7QW?ref=cns_srchrow).

## Install Docker

### Download and install

After launching your base AMI, SSH in and run the following:

```sh
# Download
curl -fsSL get.docker.com -o get-docker.sh

# Install
sh get-docker.sh

# Configure service to start on boot
sudo systemctl enable docker

# Clean up
rm get-docker.sh
```

### Configure user

It is convenient to be able to run Docker commands without `sudo`*.

```sh
sudo usermod -aG docker centos
```

> \* WARNING: Adding a user to the "docker" group will grant the ability to run containers which can be used to obtain root privileges on the docker host. Refer to [https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface) for more information.
