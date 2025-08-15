# Caddy Docker build with Netcup DNS and DuckDNS modules

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub%20-%20mwitasse%2Fcaddy--netcup--duckdns%20-%20%230db7ed?style=flat&logo=docker)](https://hub.docker.com/r/mwitassek/caddy-netcup-duckdns)
[![GitHub](https://img.shields.io/badge/GitHub%20-%20mwitasse%2Fcaddy--netcup--duckdns%20-%20%23333?style=flat&logo=github)](https://ghcr.io/mwitasse/caddy-netcup-duckdns)

[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/mwitasse/caddy-custom-builds?label=Release)](https://github.com/mwitasse/caddy-custom-builds/releases)
[![GitHub build status](https://img.shields.io/github/actions/workflow/status/mwitasse/caddy-custom-builds/build.caddy-netcup-duckdns.yml?label=Build)](https://github.com/mwitasse/caddy-custom-builds/actions/workflows/build.caddy-netcup-duckdns.yml)

This image is updated automatically by GitHub Actions when a new version of [Caddy](https://github.com/caddyserver/caddy) is released using the official [Caddy Docker](https://hub.docker.com/_/caddy) image and the following modules:
- [**Netcup DNS**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#dns-modules): for Netcup DNS-01 ACME validation support | [caddy-dns/netcup](https://github.com/caddy-dns/netcup-ddns)
- [**DuckDNS**](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#dns-modules): for DuckDNS DNS-01 ACME validation support | [caddy-dns/duckdns](https://github.com/caddy-dns/duckdns)

## Usage

Since this image built off the official Caddy Docker image, the same [volumes](https://docs.docker.com/storage/volumes/) and/or [bind mounts](https://docs.docker.com/storage/bind-mounts/), ports mapping, etc. can be used with this container. Additional [environment variables](https://caddyserver.com/docs/caddyfile/concepts#environment-variables) may be needed for the added modules. Please, refer to the repository's [README](https://github.com/mwitasse/caddy-custom-builds?tab=readme-ov-file#container-creation) file for further usage instructions.

Docker builds for all Caddy supported platforms available at the following container registries:
- [**Docker Hub**](https://hub.docker.com/r/mwitassek/caddy-netcup-duckdns) `docker pull mwitasse/caddy-netcup-duckdns:latest`
- [**GitHub Packages**](https://ghcr.io/mwitasse/caddy-netcup-duckdns) `docker pull ghcr.io/mwitasse/caddy-netcup-duckdns:latest`

### Tags

The following tags are available for the `mwitasse/caddy-netcup-duckdns` image:

- `latest`
- `<version>` (eg: `2.7.4`, including: `2.7`, `2`, etc.)

## Contributing

Feel free to contribute, request additional Caddy images with your preferred modules, and make things better by opening an [Issue](https://github.com/mwitasse/caddy-custom-builds/issues) or [Pull Request](https://github.com/mwitasse/caddy-custom-builds/pulls).

## License

Software under [GPL-3.0](https://github.com/mwitasse/caddy-custom-builds/blob/main/LICENSE) ensures users' freedom to use, modify, and distribute it while keeping the source code accessible. It promotes transparency, collaboration, and knowledge sharing. Users agree to comply with the GPL-3.0 license terms and provide the same freedom to others.