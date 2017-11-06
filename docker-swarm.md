# Docker Swarm

[Back to table of contents...](README.md)

## Docker Swarm required ports

[From a digital ocean page](https://www.digitalocean.com/community/tutorials/how-to-configure-the-linux-firewall-for-docker-swarm-on-centos-7)

The network ports required for a Docker Swarm to function properly are:

* TCP port 2376 for secure Docker client communication. This port is required for Docker Machine to work. Docker Machine is used to orchestrate Docker hosts.
* TCP port 2377. This port is used for communication between the nodes of a Docker Swarm or cluster. It only needs to be opened on manager nodes.
* TCP and UDP port 7946 for communication among nodes (container network discovery).
* UDP port 4789 for overlay network traffic (container ingress networking).

## Creating swarm managers

1. Create an EC2 instance with Docker installed
1. SSH into the EC2 instance
1. Initialize the swarm.  *Note: If you're creating swarm workers soon then you should copy the output from the `init` command.*

    ```sh
    docker swarm init
    ```

1. Create the overlay network

    ```sh
    # For example
    docker network create --driver overlay --subnet 10.0.99.0/16 your_overlay_network
    ```

## Creating swarm workers

1. Create an EC2 instance with Docker installed
1. SSH into the swarm manager *Note: If you have the join token already, skip to step 4*
1. Get the swarm's join token

    ```sh
    docker swarm join-token worker
    ```

1. SSH into the swarm worker
1. Set a sane hostname

    ```sh
    # For example
    sudo hostname swarm-worker-1
    ```

1. Join the swarm using the swarm's join token

    ```sh
    docker swarm join --token SWMTKN-1-3ecf0f96i5y7ega0ftl8jw821wem17dvsy28xppfonpqrwzw1p-9ipsvexm7nb7n1af9t1nouxi9 192.168.98.166:2377
    ```
1. SSH into the swarm manager to confirm the worker has joined the swarm

    ```sh
    $ docker node ls
    ID                            HOSTNAME                          STATUS              AVAILABILITY        MANAGER STATUS
    vfg76dmx04jys62lontdiaw18 *   ip-192-168-98-166.ec2.internal    Ready               Active              Leader
    vw375sioxfw7peufrb0vu6acc     ip-192-168-208-238.ec2.internal   Ready               Active
    ```