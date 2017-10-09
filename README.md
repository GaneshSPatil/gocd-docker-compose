# GoCD Docker Compose

Run an Alpine Linux based docker GoCD server and multiple GoCD Agents using GoCD Docker Compose.

## Usage:

### GoCD Server
Run GoCD server standalone app with Compose:
```
docker-compose -f server.yml up
```
Above command will start a GoCD server.

### GoCD Server and Alpine-3.5 GoCD Agent

Run GoCD server and a GoCD agent with Compose:
```
docker-compose -f server.yml -f agent/build.agent.yml up
```
Above command will start a GoCD server and a GoCD `build_agent` agent with default configuration.

> **Note:** Specify `GOCD_AGENT_IMAGE_REPOSITORY` under `env/build.agent.env` to specify GoCD agent image for `build_agent`. Or override the value under `.env`  for all the agents!

### Running multiple GoCD Agents

Copy `agent/build.agent.yml` and `env/build.agent.env` and create another agent configuration (say `prod.agent.yml` and `prod.agent.env`)!

Run GoCD server, a build and a prod GoCD Agent with Compose:
```
docker-compose -f server.yml -f agent/build.agent.yml -f agent/prod.agent.yml up
```
Above command will start a GoCD server, a GoCD `build_agent` and a `prod_agent` with default configuration.

### Running multiple instances of GoCD Agent.

Run GoCD server and an 2 instances GoCD `build_agent` with Compose:
```
docker-compose -f server.yml -f agent/build.agent.yml up --scale build_agent=2
```
Above command will start a GoCD server, two GoCD `build_agent`s with default configuration.

## Available configuration options
__Specify or override following variables in `.env` file.__

### GoCD Configurations

|Variable | Usage |
|---------|-------|
|*GOCD_VERSION* | Specify GoCD version to use. Defaults to `v17.10.0`. |


### GoCD Server Configurations
|Variable | Usage |
|---------|-------|
*GO_SERVER_HTTP_PORT* | Specify GoCD server HTTP port. Defaults to `8153`.
*GO_SERVER_HTTPS_PORT* | Specify GoCD server HTTPS port. Defaults to `8154`.
*GO_SERVER_SYSTEM_PROPERTIES* | Specify GoCD server system properties.
*SERVER_GO_DATA_PATH* | Specify volume mount location for `/godata`. The GoCD server will store all configuration, pipeline history database, artifacts, plugins, and logs into this mounted volume.
*SERVER_HOME_GO_PATH* | Specify volume mount location for `/home/go`. Specify secure credentials like SSH private keys and other things this mounted volume.

### GoCD Agent Configurations
|Variable | Usage |
|---------|-------|
*GOCD_AGENT_IMAGE_REPOSITORY* | Specify GoCD agent image repository name.
*GO_AGENT_SYSTEM_PROPERTIES* | Specify GoCD agent system properties.
*AGENT_AUTO_REGISTER_KEY* | Specify autoregister key for agent to be automatically approved by the server.
*AGENT_AUTO_REGISTER_RESOURCES* | Specify comma separated list of resources that the agent should be associated with.
*AGENT_AUTO_REGISTER_ENVIRONMENTS* | Specify comma separated list of environments that the agent should be associated with.
*AGENT_AUTO_REGISTER_HOSTNAME* | Specify the name of the agent when it is registered with the server
*AGENT_GO_DATA_PATH* | Specify volume mount location for `/godata`. The GoCD agent will store all configuration and logs into this mounted volume.
*AGENT_HOME_GO_PATH* | Specify volume mount location for `/home/go`. Specify secure credentials like SSH private keys and other things this mounted volume.


### Override GoCD Agent Configurations
One can Override agent Configuration specified globally in `.env` file by redefining the Configuration variables under `env/${AGNET_NAME}.agent.env`

### Volume Mounts
* Steps to specify volume mounts for GoCD server:
  1. Specify following volume mount variables under `.env`.
     - `SERVER_GO_DATA_PATH`
     - `SERVER_HOME_GO_PATH`
  2. Specify `volumes` for `gocd_server` service under `docker-compose.override.yml`.

* Steps to specify volume mounts for GoCD agent:
  1. Specify following volume mount variables under `.env` or specific `env/${AGNET_NAME}.agent.env`.
     - `AGENT_GO_DATA_PATH`
     - `AGENT_HOME_GO_PATH`
  2. Specify `volumes` for `${AGENT_NAME}_agent` service under `docker-compose.override.yml`.
> **WARNING**: Volume mounts for Agents are not supported for multiple agent instances (with `--scale` option).
