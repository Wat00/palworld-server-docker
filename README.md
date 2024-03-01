# Palworld Server on Docker
## Features
- Sandboxed environment for higher security.
- Separate stages for updating and restarting a server, so it is easier to backup files before an update.
- Micro-service architecture.
- Low customization.

## Acquiring a Docker Compose YAML
Download the `docker-compose.yml` and dependencies from [GitHub](https://github.com/Wat00/palworld-server-docker):

```
git clone https://github.com/Wat00/palworld-server-docker.git .
```
The YAML is in the `palword` folder:

```
cd palworld
```
## QuickStart
To run a server with default settings and listening on port `8211`:

```
docker compose up
```
## Step-by-step guide
### 1. Installation
#### 1.1. Downloading and installing the server

```
docker compose up steamcmd
```
#### 1.2. Reconfiguring command-line arguments
To change command-line arguments (i.e. enable community server), edit the `.env` file `COMMAND` variable.

For a full list of arguments, see [Configure the server](https://tech.palworldgame.com/settings-and-operation/arguments). If the `-port` argument is used, then the `.env` file `PALSERVER_PORT` variable should be set to the same value.
#### 1.3. Starting the server for the first time

```
docker compose up -d --no-deps palserver
```
#### 1.4. Viewing the server logs

```
docker compose logs palserver
```
#### 1.5. Changing the server configuration
1\.5.1. Retrieve the default configuration file:

```
docker compose cp palserver:/home/steam/Steam/steamapps/common/PalServer/DefaultPalWorldSettings.ini PalWorldSettings.ini
```
1\.5.2. Edit the contents of the `PalWorldSettings.ini` file (see [Configuration file](https://tech.palworldgame.com/settings-and-operation/configuration)). If the `AdminPassword` argument is changed, then the `.env` file `ADMIN_PASSWORD` variable should be set to the same value. Also, if the `RCONPort` argument is used, then the `.env` file `RCON_PORT` variable should be set to the same value.

1\.5.3. Copy the edited file to the container:

```
docker compose cp PalWorldSettings.ini palserver:/home/steam/Steam/steamapps/common/PalServer/Pal/Saved/Config/LinuxServer/
```
Changes should take effect on the next server restart.
#### 1.6. Restarting the server

```
docker compose restart palserver
```
### 2. Maintenance
#### 2.1. Saving the world data
RCON needs to be enabled in step 1.5.2 with the `RCONEnabled=True` argument.
##### 2.1.1. Non-interactively

```
docker compose run --rm rcon Save
```
##### 2.1.2. Interactively

```
docker compose run --rm rcon
```
Write `Save` and `:q`:

```
Waiting commands for palserver:25575 (or type :q to exit)
> Save
Complete Save
> :q
```
For a full list of commands, see [Commands](https://tech.palworldgame.com/settings-and-operation/commands).
#### 2.2. Cleanly shutting down the server
RCON needs to be enabled.

```
docker compose run --rm rcon Save "Shutdown 1" && docker compose wait palserver
```
#### 2.3. Force stopping the server

```
docker compose stop
```
#### 2.4. Making a backup
Docker tools which make backups of volumes can be used.

Save files are inside the smaller `palworld_saved` volume and the server installation is inside the bigger `palworld_palserver` volume.
#### 2.5. Updating the server

```
docker compose start steamcmd
```
#### 2.6. Starting the server

```
docker compose restart palserver
```
## License
[MIT](https://github.com/Wat00/palworld-server-docker/blob/master/LICENSE)
## Links
[Docker Docs](https://docs.docker.com)

[GitHub](https://github.com)

[Palworld tech guide](https://tech.palworldgame.com)

[RCON client for executing queries on game server.](https://github.com/gorcon/rcon-cli)

[SteamCMD - Valve Developer Community](https://developer.valvesoftware.com/wiki/SteamCMD)