# COTI Node Docker Installation Method

Purpose: Provide an easy method to install, upgrade and maintain testnet nodes, with automatic SSL certification, using Docker.

# Installation Instructions

## 1. Install Docker

This method requires that you have docker and docker-compose installed.

<details>
    <summary>Docker and docker-compose instructions for Ubuntu 22.04 LTS</summary>
    If you are working on Ubuntu 22.04, I suggest installing with the following commands:

```
# Install docker
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce

# Install docker-compose
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
docker compose version
```

</details>

## 2. Clone the Repository

```
git clone https://github.com/tj-wells/coti-node.git && cd coti-node
```

## 3. Define the Environment

The .env file defines the environment variables of your node, and should be specified in the format

```.env
ACTION="testnet"
SERVERNAME="<Your desired testnet URL>"
PKEY="<Your private key>"
SEED="<Your seed key>"
VERSION="X.Y.Z"
EMAIL="<Your email address>"
```

where,

- SERVERNAME is your desired testnet URL is in the form "my-node.com" (i.e. without 'http(s)://' and 'www.').
  - If you're using a subdomain, include that too, for example, "testnet.my-node.com".
- the version you wish to run to should be in the format "X.Y.Z", for example, "3.1.2". See the list below for the versions currently available for installation.

An example .env file, `.env.sample`, is provided in the repository. You may copy it to start from a valid template file: `cp .env.sample .env`.

### Available Versions

Available Coti Node Versions:

<ul>
  <li>3.1.2</li>
  <li>3.1.0</li>
</ul>

# Running Your Testnet Node

The testnet node can be run in the foreground with

```
docker-compose up
```

This lets you check the logs and make sure the node is configured correctly. Once you are confident your node is running correctly, the containers can be run in the background with

```
docker-compose up -d
```

Depending on your OS and version of docker-compose, the `docker-compose` syntax may need to be changed to `docker compose`.

# Upgrading Your Node

Your node version can be updated by following the following steps:

1. Check that the version you would like to update to is listed in the section [Available Versions](#available-versions)
2. Update the new version number in your .env file
3. Run `docker-compose up -d` to download and run the new version

# Debugging

Below is a list of common errors/problems that have been encountered when setting up the node software, and their solutions.

- `Timeout during connect (likely firewall problem)`
  - For the SSL verification to work, your server needs to be able to accept incoming connections from the internet on ports 80 and 443.
    To get the SSL certificates installed, you will need to allow all inbound connections (0.0.0.0/0) for ports 80 and 443 to your machine. The precise steps for this will vary depending on your VPS provider.

* My node repeatedly reconnects to the network
  - COTI's node manager performs health status checks on your node using port 7070.
  - To allow the node manager to connect to your node, ensure that port 7070 is accessible from the IP addresses
    - "52.59.142.53" for testnet nodes,
    - "35.157.47.86" for mainnet nodes.

If none of the information above helps, you can ask me (<a href="https://twitter.com/tomjwells">@tomjwells</a>), consult GeordieR's <a href="https://cotidocs.geordier.co.uk/" target="_blank">gitbook guide</a>, or to get help from the community, ask in the node-operators channel in the [COTI Discord server](https://discord.com/invite/wfAQfbc3Df).

# How are the Docker images built?

For complete transparency, the Docker images are built in public in <a href="https://github.com/tj-wells/coti-node-images" target="_blank">this repository</a>, and pushed to <a href="https://hub.docker.com/r/atomnode/coti-node/tags" target="_blank">this container repository</a>, to ensure that the Dockermages are fully open-source. All of the code and executions involved in the build process are automated, and fully public and transparent.

# Credits

- This method uses the official code for Coti nodes at https://github.com/coti-io/coti-node.
- Thanks to GeordieR, whose scripts assisted in developing this installation method using Docker.
- Credits to the Coti community for the vital support and guidance given to testnet and mainnet node operators.

# STAY COTI

Stay Coti. ️‍🔥

If you have questions, I hang out a lot on twitter <a href="https://twitter.com/tomjwells">@tomjwells</a>. Come and say hi and talk Coti!

<p align="center"><a href="https://twitter.com/tomjwells" target="_blank"><img src="https://cdn.discordapp.com/avatars/343604221331111946/65130831872c9daabdb0d803ce27e594.webp?size=240"></a></p>
