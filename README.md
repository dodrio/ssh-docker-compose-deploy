# ssh-docker-compose-deploy

Deploy containers via ssh and docker-compose.

## Requirements
### Destination Machine
+ POSIX shell
+ docker-compose

### Executor Machine
+ OpenSSH

## Usage

```sh
export ssh_private_key_content='...'

sdcd -h xxx.xxx.xxx.xxx \
     -p 22 \
     -u deploy \
     -k '$ssh_private_key_content' \
     -d '~/ghost-blog' \
     -t /path/to/ghost-blog-docker-compose-yml-template
```

Read more by invoking `sdcd --help`.

## Template of docker-compose.yml

Template accept `${VAR}` style placeholder, and the value is evaluated from shell environment arguments.

Create a template YAML file:

```yaml
services:
  ghost:
    image: ghost:${GHOST_VERSION_TAG}
    logging:
      driver: journald
      options:
        tag: "{{.Name}}"
    restart: always
```

Set shell environment arguments:

```sh
export GHOST_VERSION_TAG=1.6.0-alpine
```

Generated YAML file:

```yaml
services:
  ghost:
    image: ghost:1.6.0-alpine
    logging:
      driver: journald
      options:
        tag: "{{.Name}}"
    restart: always
```
