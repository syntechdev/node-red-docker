FROM mhart/alpine-node:4

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

WORKDIR /usr/src/node-red

# Add core user so we aren't running as root.
#
# We don't run as node-red because we want to mount EFS,
# and the node-red user does not exist there.
# Use the core UID.
RUN adduser -h /usr/src/node-red -D -H -u 500 core \
    && chown -R core:core /data \
    && chown -R core:core /usr/src/node-red

USER core

# package.json contains Node-RED NPM module and node dependencies
COPY package.json /usr/src/node-red/
RUN npm install

# User configuration directory volume
VOLUME ["/data"]
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json

CMD ["npm", "start", "--", "--userDir", "/data"]

