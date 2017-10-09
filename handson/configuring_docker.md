# Hands-on: Configuring Docker and Docker Swarm Cluster

## Installing Docker

Follow the [official Docker installation instructions for Ubuntu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository).

## Important Docker configuration parameters

On Ubuntu Docker is controlled by systemd. Docker configuration can be changed by [creating systemd config files](https://docs.docker.com/engine/admin/systemd/#httphttps-proxy).

To create the config files do the following:
```
mkdir -p /etc/systemd/system/docker.service.d
echo "[Service]\nExecStart=\nExecStart=/usr/bin/dockerd -H fd:// \$DOCKER_OPTS" > /etc/systemd/system/docker.service.d/docker.conf
echo "[Service]\nEnvironment=\"DOCKER_OPTS=--bip=10.10.1.1/24\"" > /etc/systemd/system/docker.service.d/env.conf
sudo systemctl daemon-reload
sudo systemctl restart docker
systemctl show --property=Environment docker
```

The configuration files will look like this:
```
~ % cat /etc/systemd/system/docker.service.d/env.conf
[Service]
Environment="DOCKER_OPTS=--bip=10.10.1.1/24"
~ % cat /etc/systemd/system/docker.service.d/docker.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// $DOCKER_OPTS
```

Explanation of the configuration parameters:

Parameter | Explanation
--- | ---
--bip | supply a specific IP address and netmask for the docker0 bridge |
--iptables | Docker will never make changes to your system iptables rules if you set --iptables=false when the daemon starts.
--label | Custom label for this host
--dns | This DNS will be used for all containers
--graph | Storage path for all the containers, their volumes etc.
--H | How to expose docker to the outside

## Configuring Docker Swarm

The official installation instructions are available on [Docker website](https://docs.docker.com/engine/swarm/).
