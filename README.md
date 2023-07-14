# docker-smokeping-master-slave

Docker image for deploying [smokeping](https://oss.oetiker.ch/smokeping/) with master-slave configuration, based on [docker-smokeping](https://github.com/linuxserver/docker-smokeping/) from [linuxserver.io](linuxserver.io)

## Usage

### docker-compose
#### Master

```
services:
  smokeping_master:
    image: ssz66666/docker-smokeping-master-slave:latest
    hostname: smokeping-master
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /path/to/smokeping/config:/config
      - /path/to/smokeping/data:/data
    ports:
      - 80:80
    restart: unless-stopped
    secrets:
      - master_secrets

secrets:
  master_secrets:
    file: /path/to/master_secrets
```

#### Slaves
```
services:
  smokeping_slave:
    image: ssz66666/docker-smokeping-master-slave:latest
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - MASTER_URL=http://example.com:80/smokeping/smokeping.cgi
      - SLAVE_NAME=slave1
    volumes:
      - config_volume:/config
      - data_volume:/data
    restart: unless-stopped
    secrets:
      - slave_secret

volumes:
  config_volume:
  data_volume:

secrets:
  slave_secret:
    file: /path/to/slave_secret
```
