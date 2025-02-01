<!-- DO NOT EDIT THIS FILE MANUALLY -->
<!-- Please read https://github.com/linuxserver/docker-openssh-server/blob/master/.github/CONTRIBUTING.md -->
[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)](https://linuxserver.io)


[![GitHub](https://img.shields.io/static/v1.svg?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&label=linuxserver.io&message=GitHub&logo=github)](https://github.com/kokofixcomputers "view the source for all of our repositories.")


This is a fork by kokofixcomputers to introduce more customizability and features. (for ubuntu)

Find me at:


* [GitHub](https://github.com/kokofixcomputers) - view the source for all of our repositories.

# [kokofixcomputers/openssh-server](https://github.com/kokofixcomputers/docker-openssh-server-fork)

[![Scarf.io pulls](https://scarf.sh/installs-badge/linuxserver-ci/linuxserver%2Fopenssh-server?color=94398d&label-color=555555&logo-color=ffffff&style=for-the-badge&package-type=docker)](https://scarf.sh)
[![GitHub Stars](https://img.shields.io/github/stars/kokofixcomputers/docker-openssh-server-fork.svg?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&logo=github)](https://github.com/kokofixcomputers/docker-openssh-server-fork)
[![GitHub Release](https://img.shields.io/github/release/kokofixcomputers/docker-openssh-server-fork.svg?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&logo=github)](https://github.com/kokofixcomputers/docker-openssh-server-fork/releases)
[![GitHub Package Repository](https://img.shields.io/static/v1.svg?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&label=linuxserver.io&message=GitHub%20Package&logo=github)](https://github.com/kokofixcomputers/docker-openssh-server-fork/packages)
[![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/openssh-server.svg?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&label=pulls&logo=docker)](https://hub.docker.com/r/kokofixcomputers/docker-openssh-server-fork)
[![Docker Stars](https://img.shields.io/docker/stars/linuxserver/openssh-server.svg?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&label=stars&logo=docker)](https://hub.docker.com/r/kokofixcomputers/docker-openssh-server-fork)
[![Jenkins Build](https://img.shields.io/jenkins/build?labelColor=555555&logoColor=ffffff&style=for-the-badge&jobUrl=https%3A%2F%2Fci.linuxserver.io%2Fjob%2FDocker-Pipeline-Builders%2Fjob%2Fdocker-openssh-server%2Fjob%2Fmaster%2F&logo=jenkins)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-openssh-server/job/master/)
[![LSIO CI](https://img.shields.io/badge/dynamic/yaml?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&label=CI&query=CI&url=https%3A%2F%2Fci-tests.linuxserver.io%2Flinuxserver%2Fopenssh-server%2Flatest%2Fci-status.yml)](https://ci-tests.linuxserver.io/linuxserver/openssh-server/latest/index.html)

[Openssh-server](https://www.openssh.com/) is a sandboxed environment that allows ssh access without giving keys to the entire server.
Giving ssh access via private key often means giving full access to the server. This container creates a limited and sandboxed environment that others can ssh into.
The users only have access to the folders mapped and the processes running inside this container.

[![openssh-server](https://upload.wikimedia.org/wikipedia/en/6/65/OpenSSH_logo.png)](https://www.openssh.com/)

## Supported Architectures

We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://distribution.github.io/distribution/spec/manifest-v2-2/#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/).

Simply pulling `lscr.io/linuxserver/openssh-server:latest` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Available | Tag |
| :----: | :----: | ---- |
| x86-64 | ✅ | amd64-\<version tag\> |
| arm64 | ✅ | arm64v8-\<version tag\> |
| armhf | ❌ | |

## Application Setup

If `PUBLIC_KEY` or `PUBLIC_KEY_FILE`, or `PUBLIC_KEY_DIR` variables are set, the specified keys will automatically be added to `authorized_keys`. If not, the keys can manually be added to `/config/.ssh/authorized_keys` and the container should be restarted.
Removing `PUBLIC_KEY` or `PUBLIC_KEY_FILE` variables from docker run environment variables will not remove the keys from `authorized_keys`. `PUBLIC_KEY_FILE` and `PUBLIC_KEY_DIR` can be used with docker secrets.

We provide the ability to set and allow password based access via the `PASSWORD_ACCESS` and `USER_PASSWORD` variables, though we as an organization discourage using password auth for public facing ssh endpoints.

Connect to server via `ssh -i /path/to/private/key -p PORT USER_NAME@SERVERIP`

Setting `SUDO_ACCESS` to `true` by itself will allow passwordless sudo. `USER_PASSWORD` and `USER_PASSWORD_FILE` allow setting an optional sudo password.

The users only have access to the folders mapped and the processes running inside this container.
Add any volume mappings you like for the users to have access to.
To install packages or services for users to access, use the LinuxServer container customization methods described [in this blog article](https://blog.linuxserver.io/2019/09/14/customizing-our-containers/).

Sample use case is when a server admin would like to have automated incoming backups from a remote server to the local server, but they might not want all the other admins of the remote server to have full access to the local server.
This container can be set up with a mounted folder for incoming backups, and rsync installed via LinuxServer container customization described above, so that the incoming backups can proceed, but remote server and its admins' access would be limited to the backup folder.

It is also possible to run multiple copies of this container with different ports mapped, different folders mounted and access to different private keys for compartmentalized access.

#### TIPS
You can volume map your own text file to `/etc/motd` to override the message displayed upon connection.
You can optionally set the docker argument `hostname`

## Key Generation

This container has a helper script to generate an ssh private/public key. In order to generate a key please run:
```
docker run --rm -it --entrypoint /keygen.sh linuxserver/openssh-server
```

Then simply follow the prompts.
The keys generated by this script are only displayed on your console output, so make sure to save them somewhere after generation.

## Usage

To help you get started creating a container from this image you can either use docker-compose or the docker cli.

>[!NOTE]
>Unless a parameter is flaged as 'optional', it is *mandatory* and a value must be provided.

### docker-compose (recommended, [click here for more info](https://docs.linuxserver.io/general/docker-compose))

```yaml
---
services:
  openssh-server:
    image: lscr.io/linuxserver/openssh-server:latest
    container_name: openssh-server
    hostname: openssh-server #optional
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - PUBLIC_KEY=yourpublickey #optional
      - PUBLIC_KEY_FILE=/path/to/file #optional
      - PUBLIC_KEY_DIR=/path/to/directory/containing/_only_/pubkeys #optional
      - PUBLIC_KEY_URL=https://github.com/username.keys #optional
      - SUDO_ACCESS=false #optional
      - PASSWORD_ACCESS=false #optional
      - USER_PASSWORD=password #optional
      - USER_PASSWORD_FILE=/path/to/file #optional
      - USER_NAME=linuxserver.io #optional
      - LOG_STDOUT= #optional
      - WELCOME_MESSAGE=Hi #optional
      - INSTALL_SCRIPT_URL=https://... #optional
    volumes:
      - /path/to/openssh-server/config:/config
    ports:
      - 2222:2222
    restart: unless-stopped
```

### docker cli ([click here for more info](https://docs.docker.com/engine/reference/commandline/cli/))

```bash
docker run -d \
  --name=openssh-server \
  --hostname=openssh-server `#optional` \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Etc/UTC \
  -e PUBLIC_KEY=yourpublickey `#optional` \
  -e PUBLIC_KEY_FILE=/path/to/file `#optional` \
  -e PUBLIC_KEY_DIR=/path/to/directory/containing/_only_/pubkeys `#optional` \
  -e PUBLIC_KEY_URL=https://github.com/username.keys `#optional` \
  -e SUDO_ACCESS=false `#optional` \
  -e PASSWORD_ACCESS=false `#optional` \
  -e USER_PASSWORD=password `#optional` \
  -e USER_PASSWORD_FILE=/path/to/file `#optional` \
  -e USER_NAME=linuxserver.io `#optional` \
  -e LOG_STDOUT= `#optional` \
  -e WELCOME_MESSAGE=Hi `#optional` \
  -e INSTALL_SCRIPT_URL=https:// `#optional` \
  -p 2222:2222 \
  -v /path/to/openssh-server/config:/config \
  --restart unless-stopped \
  lscr.io/linuxserver/openssh-server:latest
```

## Parameters

Containers are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `--hostname=` | Optionally the hostname can be defined. |
| `-p 2222:2222` | ssh port |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Etc/UTC` | specify a timezone to use, see this [list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List). |
| `-e PUBLIC_KEY=yourpublickey` | Optional ssh public key, which will automatically be added to authorized_keys. |
| `-e PUBLIC_KEY_FILE=/path/to/file` | Optionally specify a file containing the public key (works with docker secrets). |
| `-e PUBLIC_KEY_DIR=/path/to/directory/containing/_only_/pubkeys` | Optionally specify a directory containing the public keys (works with docker secrets). |
| `-e PUBLIC_KEY_URL=https://github.com/username.keys` | Optionally specify a URL containing the public key. |
| `-e SUDO_ACCESS=false` | Set to `true` to allow `linuxserver.io`, the ssh user, sudo access. Without `USER_PASSWORD` set, this will allow passwordless sudo access. |
| `-e PASSWORD_ACCESS=false` | Set to `true` to allow user/password ssh access. You will want to set `USER_PASSWORD` or `USER_PASSWORD_FILE` as well. |
| `-e USER_PASSWORD=password` | Optionally set a sudo password for `linuxserver.io`, the ssh user. If this or `USER_PASSWORD_FILE` are not set but `SUDO_ACCESS` is set to true, the user will have passwordless sudo access. |
| `-e USER_PASSWORD_FILE=/path/to/file` | Optionally specify a file that contains the password. This setting supersedes the `USER_PASSWORD` option (works with docker secrets). |
| `-e USER_NAME=linuxserver.io` | Optionally specify a user name (Default:`linuxserver.io`) |
| `-e WELCOME_MESSAGE=Hi` | Optionally specify a welcome message |
| `-e INSTALL_SCRIPT_URL=https://...` | Specify an url to download a install script that will be ran as root upon container creation. |
| `-e LOG_STDOUT=` | Set to `true` to log to stdout instead of file. |
| `-v /config` | Contains all relevant configuration files. |

## Environment variables from files (Docker secrets)

You can set any environment variable from a file by using a special prepend `FILE__`.

As an example:

```bash
-e FILE__MYVAR=/run/secrets/mysecretvariable
```

Will set the environment variable `MYVAR` based on the contents of the `/run/secrets/mysecretvariable` file.

## Umask for running applications

For all of our images we provide the ability to override the default umask settings for services started within the containers using the optional `-e UMASK=022` setting.
Keep in mind umask is not chmod it subtracts from permissions based on it's value it does not add. Please read up [here](https://en.wikipedia.org/wiki/Umask) before asking for support.

## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id your_user` as below:

```bash
id your_user
```

Example output:

```text
uid=1000(your_user) gid=1000(your_user) groups=1000(your_user)
```

## Docker Mods

[![Docker Mods](https://img.shields.io/badge/dynamic/yaml?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&label=openssh-server&query=%24.mods%5B%27openssh-server%27%5D.mod_count&url=https%3A%2F%2Fraw.githubusercontent.com%2Flinuxserver%2Fdocker-mods%2Fmaster%2Fmod-list.yml)](https://mods.linuxserver.io/?mod=openssh-server "view available mods for this container.") [![Docker Universal Mods](https://img.shields.io/badge/dynamic/yaml?color=94398d&labelColor=555555&logoColor=ffffff&style=for-the-badge&label=universal&query=%24.mods%5B%27universal%27%5D.mod_count&url=https%3A%2F%2Fraw.githubusercontent.com%2Flinuxserver%2Fdocker-mods%2Fmaster%2Fmod-list.yml)](https://mods.linuxserver.io/?mod=universal "view available universal mods.")

We publish various [Docker Mods](https://github.com/linuxserver/docker-mods) to enable additional functionality within the containers. The list of Mods available for this image (if any) as well as universal mods that can be applied to any one of our images can be accessed via the dynamic badges above.

## Support Info

* Shell access whilst the container is running:

    ```bash
    docker exec -it openssh-server /bin/bash
    ```

* To monitor the logs of the container in realtime:

    ```bash
    docker logs -f openssh-server
    ```

* Container version number:

    ```bash
    docker inspect -f '{{ index .Config.Labels "build_version" }}' openssh-server
    ```

* Image version number:

    ```bash
    docker inspect -f '{{ index .Config.Labels "build_version" }}' lscr.io/linuxserver/openssh-server:latest
    ```

## Updating Info

Most of our images are static, versioned, and require an image update and container recreation to update the app inside. With some exceptions (noted in the relevant readme.md), we do not recommend or support updating apps inside the container. Please consult the [Application Setup](#application-setup) section above to see if it is recommended for the image.

Below are the instructions for updating containers:

### Via Docker Compose

* Update images:
    * All images:

        ```bash
        docker-compose pull
        ```

    * Single image:

        ```bash
        docker-compose pull openssh-server
        ```

* Update containers:
    * All containers:

        ```bash
        docker-compose up -d
        ```

    * Single container:

        ```bash
        docker-compose up -d openssh-server
        ```

* You can also remove the old dangling images:

    ```bash
    docker image prune
    ```

### Via Docker Run

* Update the image:

    ```bash
    docker pull lscr.io/linuxserver/openssh-server:latest
    ```

* Stop the running container:

    ```bash
    docker stop openssh-server
    ```

* Delete the container:

    ```bash
    docker rm openssh-server
    ```

* Recreate a new container with the same docker run parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
* You can also remove the old dangling images:

    ```bash
    docker image prune
    ```

### Image Update Notifications - Diun (Docker Image Update Notifier)

>[!TIP]
>We recommend [Diun](https://crazymax.dev/diun/) for update notifications. Other tools that automatically update containers unattended are not recommended or supported.

## Building locally

If you want to make local modifications to these images for development purposes or just to customize the logic:

```bash
git clone https://github.com/linuxserver/docker-openssh-server.git
cd docker-openssh-server
docker build \
  --no-cache \
  --pull \
  -t lscr.io/linuxserver/openssh-server:latest .
```

The ARM variants can be built on x86_64 hardware and vice versa using `lscr.io/linuxserver/qemu-static`

```bash
docker run --rm --privileged lscr.io/linuxserver/qemu-static --reset
```

Once registered you can define the dockerfile to use with `-f Dockerfile.aarch64`.
