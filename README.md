# Devcontainer

Collection of different development containers for Visual Studio Code.

## Prerequisites

- [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension for VSCode
- [Docker](https://docs.docker.com/engine/install/)

## Features

There are dedicated containers for different use cases, with the needed tooling preinstalled. \
E.g. the Flutter devcontainer includes Flutter, Android SDK aswell as VSCode extensions and settings.

Besides that all containers have some common tools that are generally useful:

- [Docker](https://www.docker.com/)
- [Earthly](https://earthly.dev/)
- [Oh my Bash](https://github.com/ohmybash/oh-my-bash)

There is also a multi-purpose devcontainer which can be found in the root of the project. \
It includes multiple languages and tools, but beware, the image is quite big.

## Usage

Inside your project or workspace create a `.devcontainer` folder with a file called `devcontainer.json` inside.

Add the following and change the image to the devcontainer that should be used:

```json
{
	"name": "Development",
	"image": "ghcr.io/hendric-dev/devcontainer",
	"features": {
    "ghcr.io/devcontainers/features/docker-outside-of-docker:1": {
			"version": "latest",
			"moby": true
		}
	},
	"runArgs": ["--network=host"]
}
```

Upon opening the project you will be prompted to open it inside the container.

> **Warning** \
> Some folders are mounted inside the devcontainer. See the `mounts` section in the `devcontainer.json` in this project. \
> Just create an empty folder for the ones you don't want to use.

## Configuration

A lot of config is already included into the Docker image. Check out the `devcontainer.json` next to the Earthfile of the image you are using.

All settings can be overriden by the local `devcontainer.json` that was created above.

See the [Config Reference](https://containers.dev/implementors/json_reference/) for all available options.

## Build

In case it is needed to manually build the devcontainer images, it can easily be done using Earthly. \
E.g. from the `flutter` directory:

```
earthly +build
```
