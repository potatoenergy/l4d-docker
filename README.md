**Left 4 Dead Server Docker Image**
==================================

Run a Left 4 Dead server easily inside a Docker container, optimized for ARM64 (using box86).

**Supported tags**
-----------------

* `latest` - the most recent production-ready image, based on `sonroyaalmerol/steamcmd-arm64:root`

**Documentation**
----------------

### Ports
The container uses the following ports:
* `:27015 TCP/UDP` as the game transmission, pings and RCON port
* `:27005 UDP` as the client port

### Environment variables

* `L4D_ARGS`: Additional arguments to pass to the server.
* `L4D_CLIENTPORT`: The client port for the server.
* `L4D_IP`: The IP address for the server.
* `L4D_LAN`: Whether the server is LAN-only or not.
* `L4D_MAP`: The map for the server.
* `L4D_MAXPLAYERS`: The maximum number of players allowed to join the server.
* `L4D_PORT`: The port for the server.
* `L4D_SOURCETVPORT`: The Source TV port for the server.
* `L4D_TICKRATE`: The tick rate for the server.

### Directory structure
The following directories and files are important for the server:

```
📦 /home/steam
|__📁l4d-server // The server root (l4d folder name using env)
|  |__📁l4d
|  |  |__📁cfg
|  |  |  |__⚙️server.cfg
|__📃srcds_run // Script to start the server
|__📃srcds_run-arm64 // Script to start the server on ARM64
```

### Examples

This will start a simple server in a container named `l4d-server`:
```sh
docker run -d --name l4d-server \
  -p 27005:27005/udp \
  -p 27015:27015 \
  -p 27015:27015/udp \
  -e L4D_ARGS="" \
  -e L4D_CLIENTPORT=27005 \
  -e L4D_IP="" \
  -e L4D_LAN="0" \
  -e L4D_MAP="c1m1_hotel" \
  -e L4D_MAXPLAYERS="12" \
  -e L4D_PORT=27015 \
  -e L4D_SOURCETVPORT="27020" \
  -e L4D_TICKRATE="" \
  -v /home/ponfertato/Docker/l4d-server:/home/steam/left4dead-server/l4d \
  ponfertato/l4d:latest
```

...or Docker Compose:
```sh
version: '3'

services:
  l4d-server:
    container_name: l4d-server
    restart: unless-stopped
    image: ponfertato/l4d:latest
    tty: true
    stdin_open: true
    ports:
      - "27005:27005/udp"
      - "27015:27015"
      - "27015:27015/udp"
    environment:
      - L4D_ARGS=""
      - L4D_CLIENTPORT=27005
      - L4D_IP=""
      - L4D_LAN="0"
      - L4D_MAP="c1m1_hotel"
      - L4D_MAXPLAYERS="12"
      - L4D_PORT=27015
      - L4D_SOURCETVPORT="27020"
      - L4D_TICKRATE=""
    volumes:
      - ./l4d-server:/home/steam/left4dead-server/l4d
```

**Health Check**
----------------

This image contains a health check to continually ensure the server is online. That can be observed from the STATUS column of docker ps

```sh
CONTAINER ID        IMAGE                    COMMAND                 CREATED             STATUS                    PORTS                                                                                     NAMES
e9c073a4b262        ponfertato/l4d            "/home/steam/entry.sh"   21 minutes ago      Up 21 minutes (healthy)   0.0.0.0:27005->27005/udp, 0.0.0.0:27015->27015/tcp, 0.0.0.0:27015->27015/udp   distracted_cerf
```

**License**
----------

This image is under the [MIT license](LICENSE).