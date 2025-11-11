# beingtomgreen.radarr_docker

A simple ansible role for running [Radarr](https://radarr.video/) via Docker compose using the [LinuxServer.io Radarr image](https://docs.linuxserver.io/images/docker-radarr).

For additional information on how to best handle your media paths see information on [wiki.servarr.com](https://wiki.servarr.com/docker-guide#consistent-and-well-planned-paths) and [trash-guides.info](https://trash-guides.info/File-and-Folder-Structure/Hardlinks-and-Instant-Moves/).

## Installation

Given that Galaxy seems to have abandoned roles, I suggest referencing this repository directly in your projects `requirements.yml`:

```yaml
---

roles:
  - name: beingtomgreen.radarr_docker
    src: https://github.com/BeingTomGreen/ansible-role-radarr-docker.git

collections: []
```

You can then install it alongside your other requirements as normal:

```bash
ansible-galaxy install -r requirements.yml
```

## Example usage

### Basic usage

```yaml
---

- hosts: radarr
  become: true
  vars:
    radarr_docker_env_puid: 5000
    radarr_docker_env_pgid: 5000

    radarr_docker_additional_volumes:
        - '/path/to/film:/film'
        - '/path/to/usenet:/usenet'
        - '/path/to/torrents:/torrents'
  roles:
    - role: beingtomgreen.radarr_docker
```

### Want the kitchen sink?

Seriously, take a look at [`defaults/main.yml`](defaults/main.yml), it's obnoxiously commented, just for you.

```yaml
---

- hosts: radarr
  become: true
  vars:
    radarr_docker_cap_add:
      - CAP_WAKE_ALARM
      - CAP_AUDIT_CONTROL
    radarr_docker_cap_drop:
      - CAP_CHECKPOINT_RESTORE

    radarr_docker_container_name: 'radarr_container'

    radarr_docker_depends_on:
      - 'my-little service'

    radarr_docker_devices:
      - '/dev/dri:/dev/dri'
      - '/dev/kfd:/dev/kfd'

    radarr_docker_env_puid: 5000
    radarr_docker_env_pgid: 5000

    radarr_docker_env_timezone: 'Europe/London'

    radarr_docker_extra_environment_vars:
      SOME_OTHER_VAR: 'A_VALUE'

    radarr_docker_image_tag: '10.10.6'

    radarr_docker_labels:
      traefik.enable: 'true'
      traefik.http.routers.traefik-https.entrypoints: 'https'

    # Network mode
    radarr_docker_network_mode: 'service:glutun'

    # Or external networks:
    radarr_docker_external_networks:
      - 'my-little-network'

    # Or the default ports
    radarr_docker_ports:
      - '8989:8989'

    radarr_docker_pull_policy: 'weekly'

    radarr_docker_restart_policy: 'always'

    radarr_docker_additional_volumes:
      - '/path/to/film:/film'
      - '/path/to/usenet:/usenet'
      - '/path/to/torrents:/torrents'

    radarr_docker_base_directory: '/opt/radarr_docker'

    radarr_docker_update_image: False

  roles:
    - role: beingtomgreen.radarr_docker
```

## Role Variables

See [`defaults/main.yml`](defaults/main.yml) for more details.

## Requirements

- Docker & docker compose installed on the target host
- ansible_user will also need to be in the docker group

## Dependencies

- community.docker

## License

[MIT](LICENSE)

## Author Information

[Tom Green](https://github.com/BeingTomGreen)
