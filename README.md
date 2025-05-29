# Devcontainers

A collection of devcontainers. Trying to keep these up to date.
In my devcontainers I work in userspace with (variable) username `dev`.
It's more secure and on top prevents the habit of working in root space on Linux, which is something that should be prevented as much as possible in my opinion not only for security reasons, but also for stability.

For each environment you will find the following files:
```
.gitignore
devcontainer.example.json
Dockerfile
```
The job of the `.gitignore` is to prevent the actual `devcontainer.json` to be part of `git`. This is because settings can be different accross different machines or in collaboration with other devs because the container uses a user which should map to the user of the system the container runs on. This is done with the following block of the `devcontainer.example.json`:
```
"args": {
            "UID": "1000",
            "GID": "1000",
            "DOCKER_GID": "977"
        }
```
This should be filled according to the host machine (check with `id` command).

Also make sure that this block is adjusted:
```
"mounts": [
        "type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock",
        "type=bind,source=${localEnv:HOME}/.ssh,target=/home/worker/.ssh,readonly",
        "type=bind,source=${localEnv:HOME}/.gitconfig,target=/home/worker/.gitconfig,readonly"
    ]
```
This binds the `docker` socket into the container to be able to build containers while being in the devcontainer and also `ssh` and `git` configuration files to be able to use git and ssh directly from the terminal.

There will always be a decent base setup for the `zsh` as default shell for the devcontainers and typical dev tools will be provided.


## Currently available
- Latex

