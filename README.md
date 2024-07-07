# DevContainer Environment

Using Docker as a development environment can be challenging.
Here are the main issues:

1. Created/modified files are owned by root: Handling these files outside of the container is annoying!
2. Sharing X11 is difficult when the container runs on a ssh server: you cannot copy text from your console editor or run GUI programs!
3. Your dot files are not shared with the container: Life without customized dot files is tough!
4. The .ssh folder isn't shared: Accessing your github repository inside the development container is impossible!

The problems are quite frustrating.
For example, you need to allow your container to access the `.Xauthority` file on your host to make X11 sharing available by `ssh -X`.
However, the `.Xauthority` file changes with every `ssh -X` command, so if you haven't mounted your home folder, you have to stop and restart your container after connecting to your server with `ssh -X`.

This project addresses all these annoying settings and provides everything needed to run a Docker container for development purposes.
The supported services are:

1. `intel`: oneAPI container to run SYCL application
2. `nvidia`: nvidia HPC container to run CUDA application

These two servies are provided as examples.
You can freely add more services as needed.

## Prerequisites

1. direnv: install it and set it up with editing your shell's rc file: https://direnv.net/docs/hook.html 
2. docker and docker compose: https://github.com/docker/docker-install

## How to Use

```
# Build, creates, starts, and attaches to containers for a service
docker compose up --build -d <service-name>
# Executes a bash in a running container
docker compose exec  <service-name> bash
```
