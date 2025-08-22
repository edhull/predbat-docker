# Predbat Home Assistant Add-on Container Build

![Predbat Docker](./misc/predbat-docker.png)

This repository builds on the fantastic work of [nipar44](https://hub.docker.com/r/nipar44/predbat_addon) and automates the build/push of container builds of Predbat as they are released.

This image is particularly suited to those who are running Home Assistant in a container and cannot natively install Predbat as an addon. An optional Predbat helm chart is also provided for those looking to deploy Predbat alongside an existing Home Assistant deployment in a kubernetes cluster.

The builds are hosted in the Github container registry, available for anyone and are created using the appropriate `dockerfile.OS` for each distribution.

All credit for the [predbat & predbat_addon](https://github.com/springfall2008/batpred) goes to Trefor Southwell

## TLDR

```bash
docker run -d --name predbat \
  -v /path/to/local/config:/config \
  -v /etc/localtime:/etc/localtime \
  ghcr.io/edhull/predbat:latest
```
...where /path/to/local/config contains a valid apps.yaml file appropriate for your inverter/battery.

Example apps.yaml configuration can be found at https://github.com/springfall2008/batpred/tree/main/templates

---

## Available Builds and Linux Distributions

The following options are available:  
- **Noble**: A standard version closely aligned with the Predbat addon for Home Assistant.  
- **Alpine** and **Slim**: Lightweight versions optimized to reduce size.

---

## Key Notes

- Modifications are limited to the run and startup scripts, which serve as a transport mechanism for running the Predbat application.  
- The core application files remain unaltered.  
- Any Predbat-related issues should be reported in the [[Predbat GitHub repository](https://github.com/springfall2008/batpred)].

---

## Installation

You can run each build either as a self-contained Docker container or with mounted bind points for easy access to configuration files and logs.  

By default, the container operates in the root Docker context. This can be customized during installation by specifying the `predbat` user within the command or the Docker Compose file.

---

### Command-Line Installation with Mount Points

```bash
docker run -d --name predbat \
  -v /path/to/local/config:/config \
  -v /etc/localtime:/etc/localtime \
  ghcr.io/edhull/predbat:latest
```

## Steps:

1. Replace /path/to/local/config with the full path to your local configuration directory.
2. Replace tag with the desired version of the image (e.g., alpine-latest or noble).

## Docker Compose
For a more structured and easily repeatable setup, use a docker-compose.yml file.

```yaml
services:
  predbat:
    container_name: predbat
    image: ghcr.io/edhull/predbat:latest
    restart: unless-stopped
    ports:
      - 5052:5052
    user: predbat
    volumes:
      - /path/to/local/config:/config:rw
      - /etc/localtime:/etc/localtime:ro
```

