## GoCD Docker Compose

Run an Alpine Linux based docker GoCD server and multiple GoCD Agents using GoCD Docker Compose.

### Usage:

#### GoCD Server
Run GoCD server standalone app with Compose:
```
docker-compose -f server.yml up
```
Above command will start a GoCD server.

#### GoCD Server and Alpine-3.5 GoCD Agent

Run GoCD server and Alpine-3.5 GoCD agent with Compose:
```
docker-compose -f server.yml -f agent/alpine-3.5.yml up
```
Above command will start a GoCD server and an Alpine-3.5 GoCD agent with default configuration.

#### Available GoCD Agent Os flavours:
* Alpine-3.5 GoCD Agent
* Ubuntu-12.04 GoCD Agent
* Ubuntu-14.04 GoCD Agent
* Ubuntu-16.04 GoCD Agent

#### Running multiple GoCD Agents

Run GoCD server, an Alpine-3.5 GoCD agent and ubuntu-12.04 GoCD Agent with Compose:
```
docker-compose -f server.yml -f agent/alpine-3.5.yml -f agent/ubuntu-12.04.yml up
```
Above command will start a GoCD server, an Alpine-3.5 GoCD agent and an Ubuntu-12.04 GoCD Agent with default configuration.

#### Running multiple instances of GoCD Agent of specific OS flavour.

Run GoCD server and an 2 instances Alpine-3.5 GoCD agent with Compose:
```
docker-compose -f server.yml -f agent/alpine-3.5.yml up --scale gocd-agent-alpine-3.5=2
```
Above command will start a GoCD server, two Alpine-3.5 GoCD agents with default configuration.


### Available configuration options
__Specify or override following variables in `.env` file.__

#### GoCD Configurations

|Variable | Usage |
|---------|-------|
|*GOCD_VERSION* | Specify GoCD version to use. Defaults to `v17.10.0`. |


#### GoCD Server Configurations
|Variable | Usage |
|---------|-------|
*GO_SERVER_SYSTEM_PROPERTIES* | Specify GoCD server system properties.
*SERVER_GO_DATA_PATH* | Specify volume mount location for `/godata`. The GoCD server will store all configuration, pipeline history database, artifacts, plugins, and logs into this mounted volume.
*SERVER_HOME_GO_PATH* | Specify volume mount location for `/home/go`. Specifysecure credentials like SSH private keys and other things this mounted volume.

#### GoCD Agent Configurations
|Variable | Usage |
|---------|-------|
*GO_AGENT_SYSTEM_PROPERTIES* | Specify GoCD agent system properties.
*AGENT_AUTO_REGISTER_KEY* | Specify autoregister key for agent to be automatically approved by the server.
*AGENT_AUTO_REGISTER_RESOURCES* | Specify comma separated list of resources that the agent should be associated with.
*AGENT_AUTO_REGISTER_ENVIRONMENTS* | Specify comma separated list of environments that the agent should be associated with.
*AGENT_AUTO_REGISTER_HOSTNAME* | Specify the name of the agent when it is registered with the server


#### Override OS specific GoCD Agent Configurations
One can Override agent Configuration specified in `.env` file by redefining the Configuration variables under `env/gocd-agent${OS_TYPE}.env`
