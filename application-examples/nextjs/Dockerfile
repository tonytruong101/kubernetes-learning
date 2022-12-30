# syntax=docker/dockerfile:1.4

# 1. For build React app
FROM node:lts AS development

# Setting our current work directory every command 
# we instruct the dockerfile to run will be done in this directory

WORKDIR /app

# Splitting out our react dependancies into their own cached build stage
# Allows for faster docker builds

COPY package.json /app/package.json

# Instructing Docker we want to install the dependacies for this build stage
RUN npm install

# Copy contents of the root dir into the docker image /app directory
# root is specified by `.`
COPY . /app

ENV PORT=3000

# Instructs the Dockerfile what command to start this build stage with
CMD [ "npm", "start" ]

# Instructs the Dockerfile to take what was built from the `development stage`
# Tagging it as a `build` stage, execute npm run build to generate html pages

FROM development AS build

RUN npm run build

# Extra packages and setting the stage as a target reference for future-use
# Note: `dev-envs` will be referenced in our later docker-compose yml

FROM development as dev-envs
RUN <<EOF
apt-get update
apt-get install -y --no-install-recommends git
EOF

RUN <<EOF
useradd -s /bin/bash -m vscode
groupadd docker
usermod -aG docker vscode
EOF

# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /
CMD [ "npm", "start" ]

# 2. For Nginx setup
FROM nginx:alpine

# Copy config nginx
COPY --from=build /app/.nginx/nginx.conf /etc/nginx/conf.d/default.conf

# COPY apt packages

COPY --from=build /usr/bin /usr/bin

WORKDIR /usr/share/nginx/html

# Remove default nginx static assets
RUN rm -rf ./*

# Copy static assets from builder stage
COPY --from=build /app/out .

# Containers run nginx with global directives and daemon off
ENTRYPOINT ["nginx", "-g", "daemon off;"]

