#USAGE
#docker build --no-cache=true -f latest/Dockerfile https://github.com/brianr82/node-red-docker.git -t brianr82/multinodered:latest
#docker run -d -p 2001:1880 -e FLOWS=sensor_flows.json brianr82/multinodered:latest

FROM node:6

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data
#COPY settings.js /data

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
#RUN npm install node-red-contrib-cloudera
RUN npm install node-red-contrib-kafka-node-fix

# User configuration directory volume
#EXPOSE 2000

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json

CMD ["npm", "start", "--", "--userDir", "/data"]
