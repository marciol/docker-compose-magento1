# Docker compose Magento 1.9.x

This docker compose allows you to develop more easily plugins for Magento 1.9.x series.

## Requirements

* Docker >= 1.8.3
* Docker Compose

See [Docker.com](https://www.docker.com/products/docker) for more information. You can install Docker directly from [Docker installation page](https://docs.docker.com/engine/installation/)

## Getting Started

  1. First you need to add this project as a git submodule under the `.docker` directory
  2. As said above, this docker-compose setup is intended to be used as a git submodule, so you must provide an .env file on the parent directory, with the content of `.env.sample` directory, so execute:
    1. Execute: `$ cp .env.sample ../.env`.
    2. Edit the `../.env` file.
  3. Start the docker compose: `$ ./bin/start -d`.
  4. Install Magento 1.9.x: `$ ./bin/install`.
  5. Edit the `/private/etc/hosts` on macOSX or `/etc/hosts` on Linux, putting there the domain url.

## Link the plugin

  In order to make increase your life quality when developing plugins for Magento 1.9.x series, plugins files are symlinked, such that you don't need to spread files over all magento directories. Just execute:

  `$ ./bin/link`

  To test installation process and packaging you need to unlink the plugin, so execute:

  `$ ./bin/unlink`

# Debug

  Debugging is allowed after the creation of a loopback alias executing:

  `$ ./bin/initloopback`

  So you can connect to XDebug on host `10.254.254.254` port `9001`.
