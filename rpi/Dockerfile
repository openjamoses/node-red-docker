#Usage
# First Build the image on the docker host
#docker build --no-cache=true -f rpi/Dockerfile https://github.com/brianr82/node-red-docker.git -t brianr82/multinodered:latest
FROM hypriot/rpi-node:6

# add support for gpio library
RUN apt-get update
RUN apt-get install python-rpi.gpio

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN useradd --home-dir /usr/src/node-red --no-create-home node-red \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red

USER node-red

# package.json contains Node-RED NPM module and node dependencies
COPY package.json /usr/src/node-red/
COPY sensor_flows.json /usr/src/node-red/

RUN npm install
RUN npm install node-red-contrib-cloudera

# User configuration directory volume
#EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json

CMD ["npm", "start", "--", "--userDir", "/data"]
