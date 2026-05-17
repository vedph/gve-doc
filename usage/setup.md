---
title: Setup
layout: default
parent: Usage
nav_order: 1
---

# Setup

## Prerequisites

To setup the editor on a server or local machine, all what you need is [installing Docker](https://vedph.github.io/cadmus-doc/deploy/docker.html) on it.

## Editor

To run the editor in your local machine:

1. download the [Docker compose script](https://github.com/vedph/cadmus-gve-app/blob/master/docker-compose.yml) for it.
2. open a terminal window in the folder where you downloaded the script.
3. run command:

```sh
sudo docker compose up
```

>Omit `sudo` for Windows.

This will download the latest Docker images, and then start:

- a MongoDB server.
- a PostgreSQL server.
- an API service whose UI can be found at <http://localhost:5092/scalar/v1>.
- a web application running at <http://localhost:4200>.

After some seconds, some random mock data items will be seeded into the database. Once seeding is complete, point your browser to <http://localhost:4200> and use this default account to enter:

- username: `zeus`
- password: `P4ss-W0rd!`

>This is a standard Cadmus editor, so for more details about its configuration refer to [Cadmus deployment documentation](https://vedph.github.io/cadmus-doc/deploy/).

If you installed Docker Desktop, you will find the whole stack of Cadmus GVE in it. Next time, you can just start it with the play button in the UI, rather than opening the terminal and running the command.

If you want to reset and start from scratch, just delete the stack (from Docker Desktop, or by entering command `sudo docker compose down`) and recreate it as explained above.

>Note that the default script does not activate data persistence beyond the lifetime of Docker containers. So when you delete the stack containers, your data will be lost and you will start from a clean database. This is on purpose until you just play with the editor. To persist data beyond containers lifetime, you must use volumes (see Cadmus deployment documentation).
