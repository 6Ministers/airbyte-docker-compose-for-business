# Installing Airbyte with docker-compose.

## Quick Installation

**Before starting the instance, direct the domain (subdomain) to the ip of the server where Airbyte will be installed!**

## 1. Airbyte Server Requirements
From and more
- 2 CPUs
- 3 RAM 
- 15 Gb 

Run for Ubuntu 22.04

``` bash
sudo apt-get purge needrestart
```

Install docker and docker-compose:

``` bash
curl -s https://raw.githubusercontent.com/6Ministers/airbyte-docker-compose-for-business/master/setup.sh | sudo bash -s
```

If `curl` is not found, install it:

``` bash
$ sudo apt-get install curl
# or
$ sudo yum install curl
```


Download Airbyte instance:

``` bash
git clone https://github.com/airbytehq/airbyte.git
```

Go to the catalog
``` bash
cd airbyte
```

Run
``` bash
./run-ab-platform.sh
```

Stop:

``` bash
docker-compose down
```


Download Airbyte `Caddyfile`:
``` bash
wget https://raw.githubusercontent.com/6Ministers/airbyte-docker-compose-for-business/master/Caddyfile
```

In the configuration file `Caddyfile`, set the following parameters:

``` bash
https://airbyte.your-domain.com:443 {
    header Strict-Transport-Security max-age=31536000;
    reverse_proxy 127.0.0.1:8000
#	tls admin@example.org
	encode zstd gzip
	
}
```

Add to docker-compose:
``` bash
services:
  caddy:
    container_name: caddy   
    image: caddy:alpine
    restart: always
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./certs:/certs
      - ./config:/config
      - ./data:/data
      - ./sites:/srv
    network_mode: "host"
```

Default Credentials: Log in using the default username airbyte and password password. Remember to update these credentials in your .env file for security.

``` bash
# Proxy Configuration
# Set to empty values, e.g. "" to disable basic auth
BASIC_AUTH_USERNAME=your_new_username_here
BASIC_AUTH_PASSWORD=your_new_password_here
```



Then open `https://airbyte.domain.com:` to access Airbyte


## Airbyte container management

**Run Airbyte**:

``` bash
docker-compose up -d
```

**Restart**:

``` bash
docker-compose restart
```

**Restart**:

``` bash
sudo docker-compose down && sudo docker-compose up -d
```

**Stop**:

``` bash
docker-compose down
```

## Documentation


https://airbytehq.github.io/operator-guides/security

https://github.com/airbytehq/airbyte

