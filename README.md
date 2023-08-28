# OpenIM Docker Deployment

OpenIM Docker provides a stable OpenIM build and deployment solution. With various deployment options available, Docker and Docker Compose simplify the entire process.

<p align="center">
    <a href="./README.md"><b> English </b></a> •
    <a href="./README_zh-CN.md"><b> 简体中文 </b></a>
</p>

</p>

## Directory Structure

```
bashCopy codeOpenIM Docker Deployment
│
├── 📁 **build/**
│   ├── 📄 Dockerfile-server
│   └── 📄 Dockerfile-chat
│
├── 📁 **openim-server/**
│   ├── 📄 docker-compose.yml
│   └── 📁 **configs/**
│       ├── 📄 server-config.yaml
│       └── 📄 other-config.yaml
│
├── 📁 **openim-chat/**
│   ├── 📄 docker-compose.yml
│   └── 📁 **configs/**
│       ├── 📄 chat-config.yaml
│       └── 📄 other-config.yaml
│
├── 📁 **env/**
│   ├── 📄 openim-server.env
│   └── 📄 openim-chat.env
│
└── 📁 **example/**
    ├── 📄 basic-openim-server-dependency.yml
    ├── 📄 only-openim-server.yml
    └── 📄 full-openim-server-and-chat.yml
```

- `build/`: Used for building Docker images.
- `openim-server/`: Used for deploying openim-server.
- `openim-chat/`: Used for deploying openim-chat.
- `env/`: Contains Docker-compose environment variable files.
- `example/`: Contains various Docker-compose examples.

### Project Structure Notes

- For changes to `openim-server` and `openim-chat`, please contribute at https://github.com/OpenIMSDK/Open-IM-Server/ and https://github.com/OpenIMSDK/chat respectively.
- To synchronize scripts and configuration files of the two projects, we use automation tools. You only need to ensure that the files are synchronized with the original repository.
- For environment variable files and Docker-compose examples, make changes under the `env/` and `example/` directories.

## How to Use OpenIM Docker

#### 1. Obtain Images

You can obtain Docker images from the following three sources:

- [GitHub Packages](https://github.com/orgs/OpenIMSDK/packages?repo_name=Open-IM-Server)
- AliCloud (阿里云)
- Docker Hub

To ensure you get the latest version of the image, please refer to the following documents:

- [OpenIM Version Design](https://github.com/OpenIMSDK/Open-IM-Server/blob/main/docs/conversions/version.md)
- [OpenIM Image Strategy](https://github.com/OpenIMSDK/Open-IM-Server/blob/main/docs/conversions/images.md)

#### 2. Using Docker-compose

**Clone the repository:**

```
git clone https://github.com/OpenIMSDK/openim-docker openim/openim-docker && export openim=$(pwd)/openim && cd $openim/openim-docker
```

**Default launch option:**

```
docker-compose up -d
```

> **Note**: If image pulling is slow, you can choose the image from AliCloud. Both openim-server and openim-chat use the same image, just modify the image in the docker-compose.yml.

```
bashCopy code# image: ghcr.io/openimsdk/openim-server:latest
image: registry.cn-hangzhou.aliyuncs.com/openimsdk/openim-server:latest
# image: openim/openim-server:latest
```

**Custom Start-Up**

Based on your requirements, choose the appropriate Docker-compose file to start:

- **Basic Environment Dependency**:

  ```bash
  docker-compose -f example/basic-openim-server-dependency.yml up -d
  ```

- **Only OpenIM Server**:

  ```bash
  docker-compose -f example/only-openim-server.yml up -d
  ```

- **Both OpenIM Server and Chat**:

  ```bash
  docker-compose -f example/full-openim-server-and-chat.yml up -d
  ```

**Mounting:**

In the Docker Compose file, the "volumes" keyword defines named volumes. Named volumes are a storage concept in Docker that allows you to create persistent storage for a container. This means that even if the container is deleted, the data stored on that volume will not be lost.

For easy management, we use named volumes. These volumes have the same name as the services. For instance, the openim-server service uses the openim-server named volume, and the openim-chat service uses the openim-chat named volume.

View all volumes:

```bash
docker volume ls
```

View a specific volume:

```bash
docker volume inspect <volume-name>
```

Delete a named volume:

```bash
docker volume rm <volume-name>
```

Delete unused volumes:

```bash
docker volume prune
```

**Customizing Your Image**

For easy customization, we provide basic images of various distributions and architectures. The repository address is https://github.com/OpenIMSDK/openim-base-image, for easy customization.

**Test Running Status**

To see if all services have started, you can use:

```bash
docker-compose ps
```

If you find a container that hasn't started, you can view the logs of the specific service to find out the reason. For example, to view the logs of OpenIM Server:

```bash
docker-compose logs openim-server
```

**Stop**

To stop all services running by Docker-compose:

```bash
docker-compose down
```

If you used a specific `docker-compose` file, make sure to specify it in the `down` command as well.

#### 3. Tips

Ensure your Docker and Docker Compose are up-to-date to guarantee the best compatibility and performance.

## Contribution

- Fork this repository to your GitHub account.
- Clone the forked repository to your local environment.
- Create a new branch and name it after your contribution.
- Make changes where needed.
- Commit your changes and push them to your fork.
- Create a new Pull Request on GitHub.

We encourage community contributions and improvements to this project. For the specific contribution process, please refer to [CONTRIBUTING.md](https://chat.openai.com/CONTRIBUTING.md).

## License

This project uses the MIT license. For details, please refer to [LICENSE](https://chat.openai.com/LICENSE).
