# ansible-role-docker-stacks
> Configures docker stacks

This role install and deploy

- [dockge](https://github.com/louislam/dockge): A fancy, easy-to-use and reactive self-hosted docker compose.yaml stack-oriented manager
- A systemd like file to handle compose stacks under `/opt/stacks` as systemd with ease


## Requirements

* Docker should be installed in your machine, you can do it, using [geerlingguy.docker](https://github.com/geerlingguy/ansible-role-docker) as i do
* Docker should have compose plugin and not **docker-compose**

## Role Variables

Available variables are listed below, along with defautl values (see [defaults/main.yml](./defaults/main.yml))

```shell
docker_stacks_dir: "/opt/stacks"
# if you want to change the owner and group of the docker_stacks_dir
docker_stacks_owner: "{{ ansible_user }}"
# if you want to change the owner and group of the docker_stacks_dir
docker_stacks_group: "{{ ansible_user }}"
```

`docker_stacks_dir` is the directory where stacks will be deployed and configured to be used by [docker-stack@.service](./templates/docker-stack@.service.j2). [dockge](https://github.com/louislam/dockge) is deployed as a stack also here.

If you wanna change the default user used by ansible by another user present in the box, you can use both `docker_stacks_owner` and `docker_stacks_group` to make sure that directories are chown to this.

## Stacks

A stack is a folder under `docker_stacks_dir` (Defaults to `/opt/stacks`) that **should contains** a compose-like file inside.

### Manage Stacks

Once this role is installed, you can start/stop/enable/disable stacks using systemd as following:

- `enable`: systemd enable docker-stack@<name of the folder inside /opt/stacks>
- `disable`: systemd disable docker-stack@<name of the folder inside /opt/stacks>
- `start`: systemd start docker-stack@<name of the folder inside /opt/stacks>
- `stop`: systemd stop docker-stack@<name of the folder inside /opt/stacks>
- `status`: systemd status docker-stack@<name of the folder inside /opt/stacks>

### Dockge

You can start dockge as `systemctl enable --now docker-stack@dockge` using docker-stacks

```shell
user@host $ sudo systemctl enable docker-stack@dockge
Created symlink /etc/systemd/system/multi-user.target.wants/docker-stack@dockge.service → /opt/stacks/docker-stack@.service.
Created symlink /etc/systemd/system/docker-stack@dockge.service → /opt/stacks/docker-stack@.service.
```

## Shellx Integration

There's a [shellx](http://github.com/0ghny/shellx) community plugin for docker-stacks integration at [shellx-docker-stacks](https://github.com/0ghny/shellx-docker-stacks)

## Credits

* [docker-stack@.service](./templates/docker-stack@.service.j2): original idea from https://gist.github.com/mosquito/b23e1c1e5723a7fd9e6568e5cf91180f
* [dockge-compose.yaml](./templates/dockge-compose.yaml.j2): original file getted from https://github.com/louislam/dockge/blob/master/compose.yaml
