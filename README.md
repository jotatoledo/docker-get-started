# Docker - Get started

## Requirements

- Docker tools v18.09.3 or equivalent Docker desktop
- A virtual machine monitor compatible with `docker-machine`

## Setup swarm

Before setting up the swarm, its recommended to create some docker hosts using the installed virtual machine manager.
Using  `virtualbox` for example, the hosts can be created by running:

```bash
docker-machine create --driver virtualbox <host_name>
```

The host managed by `docker-machine` can be listed with:

```bash
docker-machine ls
```

### Setup swarm manager

For a machine to act as swarm manager, we need to init the swarm in it as follows:

```bash
docker-machine ssh <swarm_manager_host_name> "docker swarm init"
```

The output of this command will generate a default docker command that can be used to add machines to the swarm.

After this, we need to perfom an extra step so that the redis service can correctly persist data:

```bash
docker-machine ssh <swarm_manager_host_name> "mkdir ./data"
```

The state of the swarm can be checked by running:

```bash
docker node ls
```

### Setup swarm nodes

Machines can be added to the swarm by using the output of `docker swarm init` by running:

```bash
docker-machine ssh <host_name> <swarm_init_output>
```

## Start stack

In order to access local files when running docker commands on the swarm manager, we need to set the shell environment to talk to it.

First, run:

```bash
docker-machine env <swarm_manager_host_name>
```

This will output some env. variable values and a command on its last line that needs to be run in order
to set the target of the shell environment.

After doing this, we can deploy the stack by running:

````bash
docker stack deploy -c docker-compose.yml <stack_name>
````

For subsequent changes on the composition of the services to be applied on the stack, just run:

```bash
docker stack deploy -c docker-compose.yml <stack_name>
```

To teardown the stack, run:

```bash
docker stack rm <stack_name>
```

## Available services

- Redis
- Docker visualizer
- Flask web application