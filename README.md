# ML-DEV-TEMPLATE

[![Actions Status](https://github.com/julianlatest/ml-dev-template/actions/workflows/ci.yml/badge.svg)](https://github.com/julianlatest/ml-dev-template/actions)

 Template I use for machine learning projects.

 Inspiration drawn from:

* [microsoft/dstoolkit-devcontainers](https://github.com/microsoft/dstoolkit-devcontainers/tree/main)
* [dbpprt/ml-in-devcontainers](https://github.com/dbpprt/ml-in-devcontainers/tree/main)
* [dynamicwebpaige/codespaces-ml-template](https://github.com/dynamicwebpaige/codespaces-ml-template/tree/master)

## GETTING STARTED

1. Install [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install)

    * I prefer the latest Ubuntu distribution for compatibility
    * Install and set up [Git](https://git-scm.com/) inside WSL2
    * Enable [Git credential sharing](https://code.visualstudio.com/remote/advancedcontainers/sharing-git-credentials) (ssh-agent, ssh keys, ...)

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)

    * Set [WSL as the backend for Docker](https://docs.docker.com/desktop/wsl/)

1. Install [Visual Studio Code](https://code.visualstudio.com/)

    * Install the [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension or the all-in-one [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension pack

1. Initialize a new project based on this template

    * Clone and rename this repository inside WSL
    * Remove the `.git`-folder and `git init` a new repository
    * Optionally set the upstream remote repository (on [GitHub](https://github.com/), [GitLab](https://gitlab.com/), ...)
    * Rename the project folder(s) under the `src` directory
    * Reflect the project's folder name in the [Dockerfile](https://github.com/julianlatest/ml-dev-template/blob/main/.devcontainer/Dockerfile)
    * Modify `requirements-sys.txt`, `requirements-dev.txt` inside the `.devcontainer` directory and `requirements.txt` inside the project's directory according to your needs

1. Open the repository in Visual Studio Code from WSL using `code .` command

1. Reopen the repository inside the devcontainer

    * Either when prompted or by opening the Command Palette (<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>) and selecting `Dev Containers: Rebuild and Reopen in Container`

1. Code and commit from within the devcontainer

    * NOTE: After the container is built, the vscode user will own the repository and it will no longer by possible to edit the files directly from WSL. To make changes directly from WSL, it is necessary to run the command `sudo chown -R wsluser:wsluser .` from the repository root (replace wsluser by whatever user is running in WSL).

## TODO

* Convert to a repository template
* Remove hard coding the project name in the Dockerfile (preferably loop over all projects in the src folder when installing requirements)
* Investigate whether it is beneficial to provide a conda environment (cfr. codespaces-ml-template)
* Investigate whether it is possible for the WSL user to retain R/W rights to the repository (avoid having to run `sudo chown -R wsluser:wsluser .` when wanting to make a change from inside WSL instead of inside the devcontainer)

## TODO documentation

* Explain [common utils](https://github.com/devcontainers/features/tree/main/src/common-utils) in devcontainer.json's features
* Explain the purpose and difference between requirements-sys, requirements-dev and requirements
* Explain non-root user "bug"

  * by default the root user owns the repository, but we are the vscode user inside the devcontainer, leading to "dubious ownership" issues when the `pre-commit install` command is run
  * <https://www.kenmuse.com/blog/avoiding-dubious-ownership-in-dev-containers>
  * <https://github.com/microsoft/vscode-remote-release/issues/8656>
