# Lichess Local Development

## Instructions

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) and have it running

1. Clone this repo:

    ```
    git clone https://github.com/fitztrev/lichess-docker-compose.git
    ```

1. Setup the Lichess repos:

    ```
    cd lichess-docker-compose
    ./setup.sh
    ```

1. Start the services:

    ```
    ## start the basic services (lila, lila-ws, mongodb, redis)
    docker compose up
    ```

    Or to include optional services, depending on what you're working on, apply the appropriate profile(s):

    ```
    ## include stockfish services (for playing and analyzing)
    COMPOSE_PROFILES=stockfish docker compose up

    ## include external engine service
    COMPOSE_PROFILES=external-engine docker compose up

    ## include ALL optional services
    COMPOSE_PROFILES=stockfish,external-engine,search,images docker compose up
    ```

    Might take 5-10 minutes. Some services will start before others and you may see errors in the logs until everything comes online.

    Lila will be the last service to complete, at which point you can visit http://localhost:8080/ to see the site.

1. (Optional, but recommended) Seed your database with test data (users, games, etc):

    ```
    ./init-db.sh
    ```

## URLs

| Service               | URL                                                      |
| --------------------- | -------------------------------------------------------- |
| Main lila instance    | http://localhost:8080/                                   |
| API docs              | http://localhost:8089/                                   |
| PGN Viewer            | http://localhost:8090/                                   |
| lila-gif              | http://localhost:6175/image.gif?fen=4k3/6KP/8/8/8/8/7p/8 |
| Picfit                | http://localhost:3001/healthcheck                        |
| Mongodb manager       | http://localhost:8081/                                   |
| Elasticsearch manager | http://localhost:5601/                                   |
| Email inbox           | http://localhost:8025/                                   |

## Usage

To watch for Typescript/SCSS changes and automatically recompile:

```
docker run --rm -v $(pwd)/repos/lila:/mnt node:latest bash -c "npm install -g pnpm && /mnt/ui/build -w"
```

To restart lila (after making changes to any Scala code):

```
docker compose restart lila
```

Code formatting:

```
# lila scalafmtAll
docker run --rm -v $(pwd)/repos/lila:/mnt sbtscala/scala-sbt:eclipse-temurin-focal-17.0.5_8_1.9.1_3.3.0 bash -c "cd /mnt && sbt scalafmtAll"
```

Scalachess:

```
## compile
docker run --rm -v $(pwd)/repos/scalachess:/mnt sbtscala/scala-sbt:eclipse-temurin-focal-17.0.5_8_1.9.1_3.3.0 bash -c "cd /mnt && sbt compile"

## test
docker run --rm -v $(pwd)/repos/scalachess:/mnt sbtscala/scala-sbt:eclipse-temurin-focal-17.0.5_8_1.9.1_3.3.0 bash -c "cd /mnt && sbt test"

## package
docker run --rm -v $(pwd)/repos/scalachess:/mnt sbtscala/scala-sbt:eclipse-temurin-focal-17.0.5_8_1.9.1_3.3.0 bash -c "cd /mnt && sbt package"
```
