# Caddy Docker Custom Builds
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/mwitasse/caddy-custom-builds?label=Release)](https://github.com/mwitasse/caddy-custom-builds/releases)
[![GitHub build status](https://img.shields.io/github/actions/workflow/status/mwitasse/caddy-custom-builds/update-caddy-version.yml?label=Auto-update)](https://github.com/mwitasse/caddy-custom-builds/actions/workflows/update-caddy-version.yml)
[![License](https://img.shields.io/github/license/mwitasse/caddy-custom-builds?label=License)](https://github.com/mwitasse/caddy-custom-builds/blob/main/LICENSE)

Based on the [Caddy Custom Builds](https://github.com/serfriz/caddy-custom-builds) repo with a lot more module combinations. I decided to create my own repo for my individual needs only to also reduce the build cost.

[Caddy](https://github.com/caddyserver/caddy) takes a [modular approach](https://caddyserver.com/docs/extending-caddy) to building Docker images, allowing users to include only the [modules](https://caddyserver.com/docs/modules/) they need. This repository aims to provide flexibility and convenience to run Caddy with specific combinations of modules by providing pre-built images according to the needs and preferences of the users.

All custom images are updated automatically when a [new version](https://github.com/caddyserver/caddy/releases) of Caddy is released using the official [Caddy Docker](https://hub.docker.com/_/caddy) image. This is done by using GitHub Actions to build and push the images for all Caddy supported platforms to Docker Hub and GitHub Packages. In addition, since the update cycle of many modules is faster than Caddy's, all custom images are periodically re-built with the latest version of their respective modules on the first day of every month. Those who are already running Caddy's latest version can force the update by re-creating the container (i.e. running `docker compose up --force-recreate` if using Docker Compose).

Every [release](https://github.com/mwitasse/caddy-custom-builds/releases) lists the upstream Caddy version and the version of each module used in every build. A new release is only created when one of these versions has changed.

All commits and tags are signed with a GPG key to ensure their integrity and authenticity, and 2FA is enabled in the accounts involved in the management of this repository and the container registries.

## Builds

### Caddy Images:

- [**caddy-cloudflare**](https://github.com/mwitasse/caddy-custom-builds/tree/main/caddy-cloudflare): includes Cloudflare DNS and IPs modules.
- [**caddy-netcup-duckdns**](https://github.com/mwitasse/caddy-custom-builds/tree/main/caddy-netcup-duckdns): includes Netcup DNS and DuckDNS modules.
- [**caddy-netcup-duckdns-dockerproxy**](https://github.com/mwitasse/caddy-custom-builds/tree/main/caddy-netcup-duckdns-dockerproxy): includes Netcup DNS, DuckDNS and Docker Proxy modules.
- [**caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip**](https://github.com/mwitasse/caddy-custom-builds/tree/main/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip): includes Netcup DNS, DuckDNS, Docker Proxy, Layer4, GeoIP Filter and Replace Response modules.



### Modules:

- [**Cloudflare DNS**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#dns-modules): for Cloudflare DNS-01 ACME validation support | [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare)
- [**Cloudflare IPs**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#cloudflare-ips): to retrieve Cloudflare's current [IP ranges](https://www.cloudflare.com/ips/) | [WeidiDeng/caddy-cloudflare-ip](https://github.com/WeidiDeng/caddy-cloudflare-ip)
- [**DuckDNS**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#dns-modules): for DuckDNS DNS-01 ACME validation support | [caddy-dns/duckdns](https://github.com/caddy-dns/duckdns)
- [**Netcup DNS**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#dns-modules): for Netcup DNS-01 ACME validation support | [caddy-dns/netcup](https://github.com/caddy-dns/netcup)
- [**Docker Proxy**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#docker-proxy): enables Caddy to be used for Docker containers via labels | [lucaslorentz/caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy)
- [**Layer4**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#layer4): TCP/UDP app for Layer 4 proxying with SNI-based routing | [mholt/caddy-l4](https://github.com/mholt/caddy-l4)
- [**GeoIP Filter**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#geoip-filter) to allow or block traffic from specific regions based on [Maxmind GeoLite2 database](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data) | [porech/caddy-maxmind-geolocation](https://github.com/porech/caddy-maxmind-geolocation)
- [**Replace Response**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#replace-response): search and replace in HTTP response bodies | [caddyserver/replace-response](https://github.com/caddyserver/replace-response)

## Usage

Since all images from this repository are built off the official Caddy Docker image, the same [volumes](https://docs.docker.com/storage/volumes/) and/or [bind mounts](https://docs.docker.com/storage/bind-mounts/), ports mapping, environment variables, etc. can be used with this container. Please refer to the official [Caddy Docker](https://hub.docker.com/_/caddy) image and [docs](https://caddyserver.com/docs/) for more information on using Caddy.

Docker builds for all Caddy supported platforms are available at the following container registries:
- **Docker Hub** > `docker pull mwitassek/<caddy-build-name>:latest`
- **GitHub Packages** > `docker pull ghcr.io/mwitasse/<caddy-build-name>:latest`

To pull a specific build, replace `<caddy-build-name>` with the desired one. For example, to pull the `caddy-cloudflare` build from Docker Hub, use `docker pull mwitassek/caddy-cloudflare:latest`.

### Tags

The following tags are available for all images:

- `latest`
- `v<caddy-version>-<build_time>-custom` (eg: `v2.11.4-20261014000000-custom`), matching the tag of the corresponding [release](https://github.com/mwitasse/caddy-custom-builds/releases)

### Container Creation

Simply create the container using the `docker run` command, or a `docker-compose.yml` file including the necessary [environment variables](https://caddyserver.com/docs/caddyfile/concepts#environment-variables) depending on the modules used. The following blocks contain examples for both methods using `<caddy-build-name>` as the image name (replace it with the desired Caddy build name), and including all environment variables required by the modules listed above (some may not apply to your specific build).

#### Docker Run

```sh
docker run --rm -it \
  --name caddy \  # feel free to choose your own container name
  --restart unless-stopped \  # run container unless stopped by user (optional)
  -p 80:80 \  # HTTP port
  -p 443:443 \  # HTTPS port
  -p 443:443/udp \  # HTTP/3 port (optional)
  -v caddy-data:/data \  # volume mount for certificates data
  -v caddy-config:/config \  # volume mount for configuration data
  -v $PWD/Caddyfile:/etc/caddy/Caddyfile \  # to use your own Caddyfile
  -v $PWD/log:/var/log \  # bind mount for the log directory (optional)
  -v $PWD/srv:/srv \  # bind mount to serve static sites or files (optional)
  -e CLOUDFLARE_API_TOKEN=<token-value> \  # Cloudflare API token (if applicable)
  -e DUCKDNS_API_TOKEN=<token-value> \  # DuckDNS API token (if applicable)
  -e NETCUP_CUSTOMER_NUMBER=<number-value> \  # Netcup customer number (if applicable)
  -e NETCUP_API_KEY=<key-value> \  # Netcup API key (if applicable)
  -e NETCUP_API_PASSWORD=<password-value> \  # Netcup API password (if applicable)
  mwitassek/<caddy-build-name>:latest  # replace with the desired Caddy build name
```

The volume and bind mounts can be adjusted to meet to your needs, `$PWD` is used to reference the current working directory, but you can replace it with your preferred path. The environment variables are only required if the modules used in the build require them.

The default [Caddyfile](https://github.com/caddyserver/dist/blob/master/config/Caddyfile) that is included inside the Docker container is just a placeholder to serve a static Caddy welcome page with some useful instructions. So you will most likely want to mount your own `$PWD/Caddyfile` to configure Caddy according to your needs (the file must already exist in the specified path before creating the container).

The [restart policy](https://docs.docker.com/config/containers/start-containers-automatically/#use-a-restart-policy) can be adjusted to your needs. The policy `unless-stopped` ensures the container is always running (even at boot) unless it is explicitly stopped by the user.

#### Docker Compose

```yaml
services:
  caddy:
    image: mwitassek/<caddy-build-name>:latest  # replace with the desired Caddy build name
    # container_name: caddy  # feel free to choose your own container name
    restart: "unless-stopped"  # run container unless stopped by user (optional) 
    ports:
      - "80:80"  # HTTP port
      - "443:443"  # HTTPS port
      - "443:443/udp"  # HTTP/3 port (optional)
    volumes:
      - caddy-data:/data  # volume mount for certificates data
      - caddy-config:/config  # volume mount for configuration data
      - $PWD/Caddyfile:/etc/caddy/Caddyfile  # to use your own Caddyfile
      - $PWD/log:/var/log  # bind mount for the log directory (optional)
      - $PWD/srv:/srv  # bind mount to serve static sites or files (optional)
    environment:
      - CLOUDFLARE_API_TOKEN=<token-value>  # Cloudflare API token (if applicable)
      - DUCKDNS_API_TOKEN=<token-value>  # DuckDNS API token (if applicable)
      - NETCUP_CUSTOMER_NUMBER=<number-value>  # Netcup customer number (if applicable)
      - NETCUP_API_KEY=<key-value>  # Netcup API key (if applicable)
      - NETCUP_API_PASSWORD=<password-value>  # Netcup API password (if applicable)
volumes:
  caddy-data:
    external: true
  caddy-config:
```

Defining the data volume as [external](https://docs.docker.com/compose/compose-file/compose-file-v3/#external) ensures that `docker compose down` does not delete the volume, but you may need to create it first using `docker volume create caddy-data`. This doesn't apply to bind mounts if you opt to use them instead of volumes.

### Graceful Reloads

Caddy does not require a full restart when the `Caddyfile` is modified. Caddy comes with a [caddy reload](https://caddyserver.com/docs/command-line#caddy-reload) command which can be used to reload its configuration with zero downtime.

When running Caddy in Docker, the recommended way to trigger a config reload is by executing the `caddy reload` command in the running container. First, you'll need to determine your container ID or name. Then, pass the container ID to docker exec. The working directory is set to /etc/caddy so Caddy can find your `Caddyfile` without additional arguments.

```sh
caddy_container_id=$(docker ps | grep caddy | awk '{print $1;}')  # use your container name if different from 'caddy'
docker exec -w /etc/caddy $caddy_container_id caddy reload
```

It is possible to create an [alias](https://phoenixnap.com/kb/linux-alias-command) for the `caddy reload` command to make it more convenient to use by adding the following line to your `~/.bashrc` or `~/.zshrc` file:
    
```sh
alias caddy-reload="docker exec -w /etc/caddy $(docker ps | grep caddy | awk '{print $1;}') caddy reload"
```

Once you have added the alias to the appropriate file, you will need to source it for the changes to take effect. You can do this by running `source ~/.bashrc` or `source ~/.zshrc` in your terminal. After this, you will be able to use the `caddy-reload` alias in your terminal sessions.

## Configuration

This section aims to provide some basic information on how to configure Caddy with the modules included in the custom builds, but it is not intended to be a comprehensive guide. All the examples are based on the official [Caddyfile](https://caddyserver.com/docs/caddyfile) syntax and the modules' documentation.

### DNS Modules

To make use of the different modules that provide DNS-01 ACME validation support at the server level, set the global [acme_dns](https://caddyserver.com/docs/caddyfile/options#acme-dns) directive in your `Caddyfile` using your DNS provider's name and the respective environment variable for the API token. The example shows the use case for Cloudflare DNS with the rest of the DNS providers commented out.

```Caddyfile
{
  acme_dns cloudflare {env.CLOUDFLARE_API_TOKEN} #  for Cloudflare
  # acme_dns duckdns {env.DUCKDNS_API_TOKEN} #  for DuckDNS
  # acme_dns netcup {  # for Netcup
  #   customer_number {env.NETCUP_CUSTOMER_NUMBER}
  #   api_key {env.NETCUP_API_KEY}
  #   api_password {env.NETCUP_API_PASSWORD}
  # }
  # Please refer to the respective Caddy DNS plugin page for other DNS providers
}
```

Alternatively, you can use the [`tls`](https://caddyserver.com/docs/caddyfile/directives/tls#tls) directive at each site. See the [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare) module for additional details.

```Caddyfile
my.domain.tld {
  tls {
    dns cloudflare {env.CLOUDFLARE_API_TOKEN}  # for Cloudflare
    # dns duckdns {env.DUCKDNS_API_TOKEN}  # for DuckDNS
    # dns netcup {  # for Netcup
    #   customer_number {env.NETCUP_CUSTOMER_NUMBER}
    #   api_key {env.NETCUP_API_KEY}
    #   api_password {env.NETCUP_API_PASSWORD}
    # }
    # Please refer to the respective Caddy DNS plugin page for other DNS providers
  }
}
```

#### Creating a Cloudflare API Token

You can generate a Cloudflare API token via the Cloudflare web dashboard through the following steps:

1. Login to your Cloudflare [Dashboard](https://dash.cloudflare.com/)
2. Go to [Account Profile](https://dash.cloudflare.com/profile) > [API Tokens](https://dash.cloudflare.com/profile/api-tokens)
3. Click "Create token" (Use the "Create Custom Token" option)
4. Grant the following permissions:
   - `Zone > Zone > Read`
   - `Zone > DNS > Edit`
5. Copy the token and use it as the `CLOUDFLARE_API_TOKEN` environment variable.

#### Creating a DuckDNS API Token

To generate a DuckDNS API token, login to your [DuckDNS](https://www.duckdns.org/) account, copy the token, and use it as the `DUCKDNS_API_TOKEN` environment variable. You can recreate the token by clicking on the three vertical lines in the top right corner next to your logged in email, and selecting the recreate token option.

#### Creating a Netcup API Token

To generate a Netcup API token follow the steps from the [Netcup API docs](https://helpcenter.netcup.com/en/wiki/general/our-api). Use the `NETCUP_CUSTOMER_NUMBER`, `NETCUP_API_KEY` and `NETCUP_API_PASSWORD` environment variables in the Docker Compose/Run and `Caddyfile` configuration.

### Cloudflare IPs

To restrict access to your server only to Cloudflare's IP ranges, add the [trusted_proxies](https://caddyserver.com/docs/caddyfile/options#trusted-proxies) directive to the [global options](https://caddyserver.com/docs/caddyfile/options), under servers, in your `Caddyfile`. For additional details, refer to [trusted_proxies/cloudflare](https://caddyserver.com/docs/json/apps/http/servers/trusted_proxies/cloudflare/) documentation and [WeidiDeng/caddy-cloudflare-ip](https://github.com/WeidiDeng/caddy-cloudflare-ip) repository.

```Caddyfile
{
  servers {
    trusted_proxies cloudflare {
      interval 12h
      timeout 15s
    }
  }
}
```

### Docker Proxy

The plugin scans Docker metadata, looking for labels indicating that the service or container should be served by Caddy. Then, it generates an in-memory `Caddyfile` with site entries and proxies pointing to each Docker service by their DNS name or container IP. Every time a Docker object changes, the plugin updates the `Caddyfile` and triggers Caddy to gracefully reload with zero downtime.

Additional information and `Caddyfile` configuration examples can be found in the [lucaslorentz/caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy) repository.

### Layer4

The [caddy-l4](https://github.com/mholt/caddy-l4) app adds TCP/UDP (Layer 4) proxying to Caddy, for example to route traffic by SNI without terminating TLS. Configuration examples for the `layer4` global option can be found in the [README of the build that includes it](https://github.com/mwitasse/caddy-custom-builds/tree/main/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip#layer4---sni-based-routing) and in the [mholt/caddy-l4](https://github.com/mholt/caddy-l4) repository.

### GeoIP Filter

Allows Caddy to filter traffic based on the client's IP address location. This module needs access to the Maxmind GeoLite2 database which can be downloaded for free after creating an account. Additional information is available on [Maxmind official website](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data). You will specifically need the GeoLite2-Country.mmdb file, or the GeoLite2-City.mmdb if you're matching on subdivisions and metro codes.

Information and examples about the usage of this module can be found on the on the [Caddy website's plugin page](https://caddyserver.com/docs/modules/http.matchers.maxmind_geolocation) and the [porech/caddy-maxmind-geolocation](https://github.com/porech/caddy-maxmind-geolocation) repository.

### Replace Response

The [replace-response](https://github.com/caddyserver/replace-response) module adds an HTTP handler that searches and replaces text in response bodies. Examples are available in the [README of the build that includes it](https://github.com/mwitasse/caddy-custom-builds/tree/main/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip#response-content-filtering) and in the module's repository.

## Contributing

Feel free to contribute, request additional Caddy images with your preferred modules, and make things better by opening an [Issue](https://github.com/mwitasse/caddy-custom-builds/issues) or [Pull Request](https://github.com/mwitasse/caddy-custom-builds/pulls).

## License

Software under [GPL-3.0](https://github.com/mwitasse/caddy-custom-builds/blob/main/LICENSE) ensures users' freedom to use, modify, and distribute it while keeping the source code accessible. It promotes transparency, collaboration, and knowledge sharing. Users agree to comply with the GPL-3.0 license terms and provide the same freedom to others.