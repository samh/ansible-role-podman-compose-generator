Role Name
=========

Pass this role a hash, and it will generate a `podman-compose.yml` file.
This role is based on 
[ironicbadger.docker_compose_generator](https://galaxy.ansible.com/ironicbadger/docker_compose_generator).

This is intended to run `podman-compose` "rootless" (i.e. as a normal user).

The following structure is supported and is designed to be passed to the role using group_vars.

Requirements
------------

Podman must be configured to run rootless. See
[the rootless tutorial](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md).

pipx must be installed.

Role Variables
--------------

TODO

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

- `community.general` - the pipx module was added in `community.general` 3.8.0,
  so that version is required at minimum if `podman_compose_generator_install_podman_compose`
  is enabled.

Example Playbook
----------------

TODO

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

GPLv2

Author Information
------------------

Sam Hartsfield
