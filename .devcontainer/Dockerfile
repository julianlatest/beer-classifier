# Leverage multi-stage build and copy python binaries to cuda container
FROM python:3.11.10 as python
FROM nvidia/cuda:12.6.2-cudnn-runtime-ubuntu24.04
COPY --from=python /usr/local/ /usr/local/

# Install system requirements
COPY ./.devcontainer/requirements-sys.txt /tmp
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    $(cat tmp/requirements-sys.txt) \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* tmp/requirements-sys.txt

# Create non-root user and set the default user
ARG USERNAME=vscode
# UID 1000 is already used for ubuntu so use 1010 instead
ARG USER_UID=1010
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME 
USER $USERNAME

# Make all python tools installed by pip accesible
ENV PATH=$PATH:/home/$USERNAME/.local/bin

RUN pip install --no-cache-dir pip --upgrade

# Install dev requirements
COPY ./.devcontainer/requirements-dev.txt .
RUN pip install --no-cache-dir -r requirements-dev.txt && sudo rm requirements-dev.txt

# Install common (CPU & GPU) requirements
COPY src/common/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt && sudo rm requirements.txt

# Install project requirements
# TODO: loop over projects in src folder
COPY src/beer_classifier/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt && sudo rm requirements.txt