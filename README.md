# docker-compose files for TF2 Dedicated servers

These are some docker-compose files for the [cm2network/tf2](https://hub.docker.com/r/cm2network/tf2/) docker image to make deployment of servers a little bit easier.

## Installation

Make a folder in /var/opt for your server

``sudo mkdir /var/opt/tf2``

Open ``docker-compose.yml`` in a text editor like nano, vim, or a gui application, and paste the following docker-compose file. Edit if necessary.

```yml
version: "3.3"

services:
  tf2:
    image: cm2network/tf2:latest
    container_name: "tf2"
    network_mode: "host"

    volumes:
      - ./tf2-data:/home/steam/tf-dedicated
    
    environment:
      SRCDS_TOKEN: "CHANGEME"
      SRCDS_WORKSHOP_AUTHKEY: "CHANGEME"
      SRCDS_RCONPW: "CHANGEME"
      SRCDS_PORT: 27015
      SRCDS_MAXPLAYERS: 18
      SRCDS_HOSTNAME: "Vanilla TF2 Server"
```

**Important note:** If you end up adding more services, change the ``SRCDS_PORT`` and ``SRCDS_TV_PORT`` environment variables to ensure they don't clash with other ports on the server.

Make a directory for your server's data for persistent storage, and use ``chmod`` to allow the unprivledged container user to read & write to the directory.

```
sudo mkdir tf2-data
sudo chmod -R 777 ./tf2-data
```

You should now be able to run the server(s) with ``docker-compose up -d``.

Note that the server will **not** immediately start upon running the compose file. It takes ~5-10 minutes (depending on your download speed) to download the ~12GB of files needed for the server. Afterwards, your server should be accessible to your LAN.


Extra configuration can be found at the GitHub repo for [cm2network/tf2](https://github.com/CM2Walki/TF2/blob/master/README.md#configuration).