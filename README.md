# docker-memcache.php

A [Docker](https://docker.com/) container for [memcache.php](https://github.com/lagged/memcache.php).

## Run the container

Using the `docker` command:

    CONTAINER="memcachephpdata" && sudo docker run \
      --name "${CONTAINER}" \
      -h "${CONTAINER}" \
      -v /memcachephp/ssl/certs \
      -v /memcachephp/ssl/private \
      simpledrupalcloud/data:dev

    CONTAINER="memcachephp" && sudo docker run \
      --name "${CONTAINER}" \
      -h "${CONTAINER}" \
      -p 80:80 \
      -p 443:443 \
      --volumes-from memcachephpdata \
      -e SERVER_NAME="localhost" \
      -d \
      simpledrupalcloud/memcachephp:dev

Using the `fig` command

    TMP="$(mktemp -d)" \
      && git clone http://git.simpledrupalcloud.com/simpledrupalcloud/docker-memcachephp.git "${TMP}" \
      && cd "${TMP}" \
      && git checkout dev \
      && sudo fig up

## Connect directly to Memcached server by linking with another Docker container

    CONTAINER="memcachephpdata" && sudo docker run \
      --name "${CONTAINER}" \
      -h "${CONTAINER}" \
      -v /memcachephp/ssl/certs \
      -v /memcachephp/ssl/private \
      simpledrupalcloud/data:dev

    CONTAINER="memcachephp" && sudo docker run \
      --name "${CONTAINER}" \
      -h "${CONTAINER}" \
      -p 80:80 \
      -p 443:443 \
      --volumes-from memcachephpdata \
      --link memcached:memcached \
      -e SERVER_NAME="localhost" \
      -d \
      simpledrupalcloud/memcachephp:dev

## Build the image

    TMP="$(mktemp -d)" \
      && git clone http://git.simpledrupalcloud.com/simpledrupalcloud/docker-memcachephp.git "${TMP}" \
      && cd "${TMP}" \
      && git checkout dev \
      && sudo docker build -t simpledrupalcloud/memcachephp:dev . \
      && cd -

## Back up memcache.php data

    sudo docker run \
      --rm \
      --volumes-from memcachephpdata \
      -v $(pwd):/backup \
      simpledrupalcloud/data:dev tar czvf /backup/memcachephpdata.tar.gz /memcachephp/ssl/certs /memcachephp/ssl/private

## Restore memcache.php data from a backup

    sudo docker run \
      --rm \
      --volumes-from memcachephpdata \
      -v $(pwd):/backup \
      simpledrupalcloud/data:dev tar xzvf /backup/memcachephpdata.tar.gz

## License

**MIT**
