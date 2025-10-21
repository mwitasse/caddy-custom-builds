# Caddy Docker build with Netcup DNS, DuckDNS, Docker Proxy, Layer4 and GeoIP Filter modules

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub%20-%20mwitasse%2Fcaddy--netcup--duckdns--dockerproxy--layer4--replace--geoip%20-%20%230db7ed?style=flat&logo=docker)](https://hub.docker.com/r/mwitassek/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip)
[![GitHub](https://img.shields.io/badge/GitHub%20-%20mwitasse%2Fcaddy--netcup--duckdns--dockerproxy--layer4--replace--geoip%20-%20%23333?style=flat&logo=github)](https://ghcr.io/mwitasse/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip)

[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/mwitasse/caddy-custom-builds?label=Release)](https://github.com/mwitasse/caddy-custom-builds/releases)
[![GitHub build status](https://img.shields.io/github/actions/workflow/status/mwitasse/caddy-custom-builds/build.caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip.yml?label=Build)](https://github.com/mwitasse/caddy-custom-builds/actions/workflows/build.caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip.yml)

This image is updated automatically by GitHub Actions when a new version of [Caddy](https://github.com/caddyserver/caddy) is released using the official [Caddy Docker](https://hub.docker.com/_/caddy) image and the following modules:

## Included Modules

### DNS Modules
- [**Netcup DNS**](https://github.com/caddy-dns/netcup): for Netcup DNS-01 ACME validation support
- [**DuckDNS**](https://github.com/caddy-dns/duckdns): for DuckDNS DNS-01 ACME validation support

### Proxy & Routing Modules
- [**Docker Proxy**](https://github.com/lucaslorentz/caddy-docker-proxy): enables Caddy to be used for Docker containers via labels
- [**Layer4 (caddy-l4)**](https://github.com/mholt/caddy-l4): TCP/UDP app for Layer 4 proxying with SNI-based routing

### Security & Filtering Modules
- [**MaxMind GeoIP**](https://github.com/porech/caddy-maxmind-geolocation): HTTP handler for geolocation-based filtering using MaxMind databases
- [**Replace Response**](https://github.com/caddyserver/replace-response): HTTP middleware for search and replace in response bodies (content filtering)

## Usage

Since this image is built off the official Caddy Docker image, the same [volumes](https://docs.docker.com/storage/volumes/) and/or [bind mounts](https://docs.docker.com/storage/bind-mounts/), ports mapping, etc. can be used with this container.

Docker builds for all Caddy supported platforms available at the following container registries:
- [**Docker Hub**](https://hub.docker.com/r/mwitassek/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip) `docker pull mwitasse/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip:latest`
- [**GitHub Packages**](https://ghcr.io/mwitasse/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip) `docker pull ghcr.io/mwitasse/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip:latest`

### Tags

The following tags are available for the `mwitasse/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip` image:

- `latest`
- `<version>` (eg: `2.10.2`, including: `2.10`, `2`, etc.)

## Module Configuration Examples

### Layer4 - SNI-based Routing

Route traffic based on SNI (Server Name Indication) without SSL termination:

```caddyfile
{
    layer4 {
        :443 {
            @domain1 tls sni *.domain1.com
            route @domain1 {
                proxy {
                    upstream server1:443
                }
            }
            
            @domain2 tls sni *.domain2.com
            route @domain2 {
                proxy {
                    upstream server2:443
                }
            }
        }
    }
}

# Regular HTTPS config for local termination
:8443 {
    *.domain1.com {
        reverse_proxy backend:80
    }
}
```

### MaxMind GeoIP Filtering

Block or allow traffic based on country codes:

```caddyfile
{
    order maxmind_geolocation before basicauth
}

example.com {
    maxmind_geolocation {
        db_path "/path/to/GeoLite2-Country.mmdb"
        allow_countries DE AT CH
    }
    
    respond "Hello from allowed country!"
}
```

### DNS-01 ACME Challenge with Netcup

```caddyfile
*.example.com {
    tls {
        dns netcup {
            customer_number {env.NETCUP_CUSTOMER_NUMBER}
            api_key {env.NETCUP_API_KEY}
            api_password {env.NETCUP_API_PASSWORD}
        }
    }
    
    reverse_proxy backend:80
}
```

### Docker Compose Example with All Features

```yaml
version: '3.8'

services:
  caddy:
    image: mwitasse/caddy-netcup-duckdns-dockerproxy-layer4-replace-geoip:latest
    # container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"  # For HTTP/3
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
      - ./geoip:/geoip:ro  # For MaxMind databases
    environment:
      - NETCUP_CUSTOMER_NUMBER=${NETCUP_CUSTOMER_NUMBER}
      - NETCUP_API_KEY=${NETCUP_API_KEY}
      - NETCUP_API_PASSWORD=${NETCUP_API_PASSWORD}
      - CADDY_DOCKER_MODE=server
    networks:
      - caddy

  whoami:
    image: traefik/whoami
    labels:
      caddy: whoami.example.com
      caddy.reverse_proxy: "{{upstreams 80}}"
      caddy.tls.dns: netcup
    networks:
      - caddy

networks:
  caddy:
    external: true
```

## Environment Variables

### Netcup DNS
- `NETCUP_CUSTOMER_NUMBER`: Your Netcup customer number
- `NETCUP_API_KEY`: Your Netcup API key
- `NETCUP_API_PASSWORD`: Your Netcup API password

### DuckDNS
- `DUCKDNS_API_TOKEN`: Your DuckDNS API token

### Docker Proxy
- `CADDY_DOCKER_MODE`: Set to `server` to enable docker-proxy mode (default: `controller`)

## Advanced Use Cases

### Multi-Server SNI Routing

Perfect for routing multiple domains to different servers behind a single public IP:

```caddyfile
{
    layer4 {
        :443 {
            # Route domain1.com to Server 1
            @domain1 tls sni *.domain1.com
            route @domain1 {
                proxy {
                    upstream 192.168.1.10:443
                }
            }
            
            # Route domain2.com to Server 2
            @domain2 tls sni *.domain2.com
            route @domain2 {
                proxy {
                    upstream 192.168.1.20:443
                }
            }
            
            # Default route for everything else
            route {
                proxy {
                    upstream 192.168.1.30:443
                }
            }
        }
    }
}
```

### Geographic Content Filtering

Restrict access based on visitor location:

```caddyfile
{
    order maxmind_geolocation before respond
}

api.example.com {
    maxmind_geolocation {
        db_path "/geoip/GeoLite2-Country.mmdb"
        # Only allow EU countries
        allow_countries DE FR IT ES NL BE AT CH LU
    }
    
    reverse_proxy backend:8080
}

blocked.example.com {
    maxmind_geolocation {
        db_path "/geoip/GeoLite2-Country.mmdb"
        # Block specific countries
        deny_countries CN RU
    }
    
    reverse_proxy backend:8080
}
```

### Response Content Filtering

Filter and replace content in HTTP responses:

```caddyfile
{
    order replace after encode
}

example.com {
    reverse_proxy backend:8080
    
    replace {
        # Remove sensitive information from responses
        stream {
            "SECRET_TOKEN_.*" "REDACTED"
        }
    }
}

api.example.com {
    reverse_proxy backend:8080
    
    replace {
        # Replace API endpoint URLs
        "http://old-api.internal" "https://api.example.com"
        
        # Replace in JSON responses
        `"deprecated":true` `"deprecated":false`
    }
}
```

## Getting MaxMind GeoIP Databases

To use the GeoIP filtering, you need MaxMind databases:

1. Sign up for a free account at [MaxMind](https://www.maxmind.com/en/geolite2/signup)
2. Download the GeoLite2 Country database
3. Mount the database file in your container: `-v ./GeoLite2-Country.mmdb:/geoip/GeoLite2-Country.mmdb:ro`

## Contributing

Feel free to contribute, request additional Caddy images with your preferred modules, and make things better by opening an [Issue](https://github.com/mwitasse/caddy-custom-builds/issues) or [Pull Request](https://github.com/mwitasse/caddy-custom-builds/pulls).

## License

Software under [GPL-3.0](https://github.com/mwitasse/caddy-custom-builds/blob/main/LICENSE) ensures users' freedom to use, modify, and distribute it while keeping the source code accessible. It promotes transparency, collaboration, and knowledge sharing. Users agree to comply with the GPL-3.0 license terms and provide the same freedom to others.

## Module Links

- [Caddy Documentation](https://caddyserver.com/docs/)
- [caddy-dns/netcup](https://github.com/caddy-dns/netcup)
- [caddy-dns/duckdns](https://github.com/caddy-dns/duckdns)
- [caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy)
- [caddy-l4 (Layer4)](https://github.com/mholt/caddy-l4)
- [caddy-maxmind-geolocation](https://github.com/porech/caddy-maxmind-geolocation)
- [replace-response](https://github.com/caddyserver/replace-response)