## GoCD Docker Compose

Run an Alpine Linux based docker GoCD server and multiple GoCD Agents using GoCD Docker Compose.


### Usage:

Run GoCD app with Compose:
```
docker-compose up
```

Above command will start a GoCD server and a GoCD agent with default configuration.

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
*GOCD_AGENT_IMAGE* | Specify GoCD agent image to use. Defaults to `gocd/gocd-agent-alpine-3.5`.
*GO_AGENT_SYSTEM_PROPERTIES* | Specify GoCD agent system properties.
*AGENT_AUTO_REGISTER_KEY* | Specify autoregister key for agent to be automatically approved by the server.
*AGENT_AUTO_REGISTER_RESOURCES* | Specify comma separated list of resources that the agent should be associated with.
*AGENT_AUTO_REGISTER_ENVIRONMENTS* | Specify comma separated list of environments that the agent should be associated with.
*AGENT_AUTO_REGISTER_HOSTNAME* | Specify the name of the agent when it is registered with the server


---

## NOTE:
__Deploying services with multiple replicas is not yet supported in `docker compose`.
To run multiple GoCD Agents specify replicas count and run docker compose on swarm cluster.__

```
 $(cat .env | grep ^[A-Z] | xargs) docker stack deploy --compose-file=docker-compose.yml couchbase
```
