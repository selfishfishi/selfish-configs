# Docker
- `docker build -t dockertag .`: builds an image from current dir. needs to have a Dockerfile
- `docker run -d -p 80:80 --name container-name container-tag`: runs a docker image from tag and names it
- `docker ps`: docker information
- `docker tag container-tag selfishfish/container-tag` and `docker push selfishfish/container-tag` pushes image to docker hub
- `docker exec -it <container-id> /bin/sh` runs a command interactively in the container
- `docker stop <container-id>` and `docker rm <container-id>` can be done in one command: `docker rm <container-id>`
- `docker image ls`: list all the avilable images 
- `docker logs --follow <contain-id>`: follow the logs
- `docker volume create [name]`: create a named volume
- `docker run -dp 80:80 -v [volume-name]:[path-in-container] [container-name]` or `docker run -dp 80:80 -v [path-in-filesystem]:[path-in-container]` for bind volumes
- `docker network create network-name` to create a network and docker run `--network [network-name] --network-alias [container-name-on-network]...` to run the container on your network
- `docker scan [image-name]` to scan for vulnerabilities
- Always combine RUN apt-get update with apt-get install
- Always combine RUN apt-get update with apt-get install -y

# Docker Compose
- `docker compose up -d` run everything in compose in the background
- `docker compose down --volumes` also removes the volumes
- You can have multiple dockercompose files for different environments or you can merge them together on the command line
-  `docker-compose config`: validates the docker compose file
- `docker compose up --build`: rebuilds images, this needs to be run if source code changes, docker compose doesn't figure it out by itself
- Unless you actuially bind the volume to your source cood and that you don't need recompliation (which you need in go): 
```
    volumes:
      - .:/code
```
- You can extend a Compose file using the extends field or by creating multiple Compose files. See extends for more details.
- You can see your docker compose with env varilables replaced in by `docker compose convert`
	- These env variables are not automatically passed to the container, just the docker compose command. To pass env variables you can either add them under the environment flag in the yaml file or refer to them under `env_file` flag
- It is a good idea to set default values for env variables inside the docker-compose yaml. There is a special syntax for that.
- You can assign services to profiles with `profiles` keyword. You can then enable a specficic profile by `docker compose --profile debug up` and only services in that profile will come up
- You can pass multiple compose files and they will be merged by docker compose: `docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d`. This is usueful in scenarios such as different environments and admin tasks

# General 
- use multistage builds to keep your image size small 
- use `secrets` to store sensitive application data and configs for non-sensitive data
- You can have .env file and a secret file that docker compose automatically reads from
- If a service can run without privileges, use USER to change to a non-root user. Start by creating the user and group in the Dockerfile with something like:
