FROM python:3.10.10-buster

###########################################
# Upgrade the packages
###########################################
# Download latest listing of available packages:
RUN apt-get -y update
# Upgrade already installed packages:
RUN apt-get -y upgrade
# Install a new package:

###########################################
# install tree package
###########################################
# Install a new package:
RUN apt-get -y install tree


############################################
# create group and user
############################################

ARG UNAME=simha
ARG UID=1000
ARG GID=1000


RUN cat /etc/passwd

# create group
RUN groupadd -g $GID $UNAME

# create a user with userid 1000 and gid 100
RUN useradd -u $UID -g $GID -m -s /bin/bash $UNAME
# -m creates home directory

# change permissions of /home/simha to 1000:100
RUN chown $UID:$GID /home/simha


#############################################
# install pipenv
############################################
ENV PIPENV_VENV_IN_PROJECT=1

# ENV PIPENV_VENV_IN_PROJECT=1 is important: it causes the resuling virtual environment to be created as /app/.venv. Without this the environment gets created somewhere surprising, such as /root/.local/share/virtualenvs/app-4PlAip0Q - which makes it much harder to write automation scripts later on.

RUN python -m pip install --upgrade pip

RUN pip install --no-cache-dir pipenv

###########################################
# change working dir and user
###########################################

USER $UNAME

RUN mkdir -p /home/$UNAME/app
WORKDIR /home/$UNAME/app



# Create docker image as below
# cd /home/extra/simha/Docker_images/PYTHON_3_10_10_CUSTOM
# docker build -t python3.10.10_custom_build:latest --file Dockerfile .

# run image
# docker run --rm -it python3.10.10_custom_build:latest /bin/bash