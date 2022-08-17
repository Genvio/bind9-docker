<img width="450px" alt="BIND9 logo" src="https://www.isc.org/images/isclogos/Bind_9_ISC_Blue.jpg">

# BIND9-docker

ISC BIND 9 Docker container for Genvio Inc, based on Ubuntu 22.04 LTS image. This free Docker is maintained on a best-effort basis.

You need to properly mount the following volumes:

`/etc/bind` - for configuration, your named.conf lives here  
`/var/cache/bind` - this is the working directory, e.g. options `{ directory "/var/cache/bind"; };`  
`/var/lib/bind` - this is usually the place where the secondary zones are placed  
`/var/log` - for logfiles  

## Quickstart

### Recursive DNS Server

#### BIND 9.19 (current stable)

    docker run \
            --name=bind9 \
            --restart=always \
            --publish 53:53/udp \
            --publish 53:53/tcp \
            --publish 127.0.0.1:953:953/tcp \
            ghcr.io/genvio/bind9-docker:latest


### Authoritative DNS Server

Here you will actually want to provide the desired configuration in /etc/bind/named.conf and primary zones, etc… (e.g. it’s not magic, you will have to configure it).

#### BIND 9.19 (current stable)

    docker run \
            --name=bind9 \
            --restart=always \
            --publish 53:53/udp \
            --publish 53:53/tcp \
            --publish 127.0.0.1:953:953/tcp \
            --volume /etc/bind \
            --volume /var/cache/bind \
            --volume /var/lib/bind \
            --volume /var/log \
            ghcr.io/genvio/bind9-docker:latest

### Basic Configuration

#### Recursive DNS server
The recursive DNS server doesn't need any configuration.

#### Authoritative DNS server

    options {
            directory "/var/cache/bind";
            listen-on { 127.0.0.1; };
            listen-on-v6 { ::1; };
            allow-recursion {
                    none;
            };
            allow-transfer {
                    none;
            };
            allow-update {
                    none;
            };
    };

    zone "example.com." {
            type primary;
            file "/var/lib/bind/db.example.com";
            notify explicit;
    };


### Documentation

Documentation for BIND (9.16 and later versions only) is available at [ReadTheDocs](https://bind9.readthedocs.io/en/v9_18_5/index.html) at the ISC Documents Website.  
BIND9 is licensed under the MPL2.0 license by ISC.
