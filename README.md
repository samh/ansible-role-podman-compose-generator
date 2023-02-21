Podman Compose Generator
========================

Pass this role a hash, and it will generate a `podman-compose.yml` file.
This role is based on 
[ironicbadger.docker_compose_generator](https://galaxy.ansible.com/ironicbadger/docker_compose_generator).

This is intended to run `podman-compose` "rootless" (i.e. as a normal user).

The following structure is supported and is designed to be passed to the role using group_vars.

Requirements
------------

Podman must be configured to run rootless. See
[the rootless tutorial](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md).

If `podman_compose_generator_install_podman_compose` is enabled, then
[pipx](https://pypa.github.io/pipx/) must be installed.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    podman_compose_generator_user: ""
    podman_compose_generator_group: "{{ podman_compose_generator_user }}"

The user and group who runs rootless podman.

    podman_compose_generator_install_podman_compose: true

If true, installs `podman-compose` using `pipx`.

    podman_compose_generator_podman_compose_bin: "/home/{{ podman_user }}/.local/bin/podman-compose"

Path to the `podman-compose` executable.

    podman_compose_generator_containers: []

A list of container definitions.

    podman_compose_generator_compose_template: "compose.yml.j2"

The template used to generate the compose file. May be used instead of
`podman_compose_generator_containers` to deploy a compose file directly.

    podman_compose_generator_compose_file: "~/compose.yml"

The path/filename of the generated compose file.

    podman_compose_generator_schema_version: "3.5"

The `version` used in the compose file.

    podman_compose_generator_add_service: true

If true, add a systemd user service to run this `podman-compose` file.

    podman_compose_generator_service_name: podman-compose

The name of the systemd unit (`.service` will be added to it).

    podman_compose_generator_service_description: "Start {{ podman_compose_generator_service_name }}"

The "Description" added to the systemd unit file.

    podman_compose_generator_service_dir: "~/.config/systemd/user"

The directory to create the systemd unit file.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

- `community.general` - the pipx module was added in `community.general` 3.8.0,
  so that version is required at minimum if `podman_compose_generator_install_podman_compose`
  is enabled.

Example Playbook
----------------

```yaml
- hosts: servers
  tasks:
    - name: configure podman-compose
      tags: [container, podman, compose]
      block:
        - import_role:
            name: samh.podman_compose_generator
```

To generate multiple compose files, pass in these variables for each instance:

```yaml
- hosts: servers
  tasks:
    - name: configure podman-compose for another service
      tags: [container, podman, compose, another-service]
      block:
        - import_role:
            name: samh.podman_compose_generator
          vars:
            podman_compose_generator_containers: "{{ another_service_containers }}"
            podman_compose_generator_compose_file: "~/another-service-compose.yml"
            podman_compose_generator_service_name: "another-service"
```

Example Definitions
-------------------

Variables are normally added to `group_vars` / `host_vars`.

```yaml
podman_compose_generator_user: my_username
#podman_compose_generator_group: my_group # if different from username

# Install using pipx if enabled
podman_compose_generator_install_podman_compose: true
# Create a systemd user service
podman_compose_generator_add_service: true

podman_compose_generator_containers:
  - service_name: duplicati
    # Set 'active' to false to exclude this container from the config without
    # having to comment it out.
    active: true
    description: Duplicati service for restoring old backups
    image: lscr.io/linuxserver/duplicati:latest
    container_name: duplicati
    environment:
      - PUID=0
      - PGID=0
      - TZ=America/New_York
    volumes:
      - "{{ appdata_path }}/duplicati:/config"
      - /media/backups:/backups
    ports:
      - 8200:8200
    restart: unless-stopped
```

License
-------

GPLv2

Author Information
------------------

Sam Hartsfield
