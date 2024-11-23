# Devcontainer Playground

## Summary

This Devcontainer setup is designed to create a development environment with a variety of tools and languages pre-installed. The Dockerfile starts from a .NET SDK base image and installs several packages including development tools for languages like Clojure, Groovy, Kotlin, Python, Ruby, and Vala. It also installs utilities like git, htop, neovim, and fzf.

Additionally, the Dockerfile sets up the Crystal programming language and ANTLR, a powerful parser generator. It configures the environment variables for .NET tools and ANTLR.

For a more detailed list of installed packages, refer to the [Dockerfile](.devcontainer/Dockerfile).

The devcontainer.json includes a postCreateCommand that sets up aliases for ANTLR and clones a repository to further configure the user environment. This setup ensures that the development container is ready with all necessary tools and configurations for a smooth development experience in the way I like it.

## Build multi-platform image on MacOS / AppleSilicon

reference: <https://docs.docker.com/build/building/multi-platform/>

One way to build a multi-platform image is to create and use a custom builder:

```bash
docker buildx create \
  --name multi-builder \
  --driver docker-container \
  --bootstrap --use
```

Then, build the image using the custom builder. I am also pushing the image to my private registry ciry.wsmy.de:

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  --tag ciry.wsmy.de/dev-playground:latest \
  --push .
```

## Publish to docker.io

```bash
skopeo login docker.io/wstein
skopeo copy --multi-arch=all docker://ciry.wsmy.de/dev-playground:latest docker://docker.io/wstein/dev-playground:latest
```
