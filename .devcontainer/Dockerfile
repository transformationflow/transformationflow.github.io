FROM mcr.microsoft.com/devcontainers/python:1-3.11-bullseye
# Define the path to the virtualenv to work with
ARG ENV_PATH="/home/vscode/venv"
# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi
# [Optional] Uncomment this section to install additional OS packages.
# Install the xz-utils package
RUN apt-get update && apt-get install -y xz-utils
RUN apt-get install -y pngquant
RUN sudo apt-get install -y libcairo2-dev libfreetype6-dev libffi-dev libjpeg-dev libpng-dev libz-dev
# x x  x x x x
#COPY requirements.txt /tmp/pip-tmp/
#RUN su vscode -c "python -m venv ${ENV_PATH}" \
#   && ${ENV_PATH}/bin/pip3 --disable-pip-version-check --no-cache-dir \
#   install -r /tmp/pip-tmp/requirements.txt \
#   && rm -rf /tmp/pip-tmp