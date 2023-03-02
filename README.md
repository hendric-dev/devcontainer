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

Create a `.devcontainer` inside your project or workspace. \
Now create a symlink from the desired `devcontainer.json` into that folder. E.g. for a Flutter project:

```sh
ln -s <path to devcontainer repo>/flutter/devcontainer.json <path to flutter project>/.devcontainer/devcontainer.json
```

Upon opening the project you will be prompted to open it inside the container.

## Build

In case it is needed to manually build the devcontainer images, it can easily be done using Earthly. \
E.g. from the `flutter` directory:

```
earthly +build
```
