# Cartesi Rollups Node Test Environment

Enviroment for testing the Cartesi Rollups Node v2. The repo consists of docker compose files and instructions to run an environment for applications.

## Set up

Either clone the repo:

```shell
git clone git@github.com:Mugen-Builders/node-test-environment.git
cd node-test-environment
```

or get the files selectively:

```shell
mkdir my-test-environment && cd my-test-environment
wget https://raw.githubusercontent.com/Mugen-Builders/node-test-environment/refs/heads/main/docker-compose.yaml
wget https://raw.githubusercontent.com/Mugen-Builders/node-test-environment/refs/heads/main/docker-compose-devnet.yaml
```

Then create a `.env`  file with the definitions of the chain and wallet you are using. You can set other `CARTESI_*` environments variables here too

```shell
CARTESI_AUTH_PRIVATE_KEY=
CARTESI_BLOCKCHAIN_ID=
CARTESI_BLOCKCHAIN_HTTP_ENDPOINT=
CARTESI_BLOCKCHAIN_WS_ENDPOINT=
```

Use an alias to simplify the command:

```shell
alias node-compose="docker compose --env-file .env -f docker-compose.yaml"
```

Note: use the `docker-compose-devnet.yaml` file is used to override the compose file and set a local devnet:

```shell
alias node-compose="docker compose -f docker-compose.yaml -f docker-compose-devnet.yaml"
```

## Usage

Start the environment:

```shell
node-compose up -d
```

Deploy the application:

```shell
node-compose run --rm -iT deployer deployer.sh - <<< '{
    "name":<name of the app>,
    "snapshot_url":<url of the application image>,
    "snapshot_sha256sum":<sha256 checksum of the application image>
}'
```

E.g. Deploy Rives Barebones Doom:

```shell
node-compose run --rm -iT deployer deployer.sh - <<< '{
    "name":"rives-barebones-doom",
    "snapshot_url":"https://github.com/lynoferraz/rives-barebones-doom/releases/download/v0.0.4/rives-barebones-doom-snapshot.tar.gz",
    "snapshot_sha256sum":"6f5b314ddd7b8ea10a20a6a321e29f26f6373ee6ae4e1c87487d84f26c7bbd34"
}'
```

Finally, stop the environment with

```shell
node-compose down -v
```
