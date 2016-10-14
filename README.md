# kafka-topics

[![release](http://github-release-version.herokuapp.com/github/landoop/kafka-topics-ui/release.svg?style=flat)](https://github.com/landoop/kafka-topics-ui/releases/latest)
[![docker](https://img.shields.io/docker/pulls/landoop/kafka-topics-ui.svg?style=flat)](https://hub.docker.com/r/landoop/kafka-topics-ui/)
[![Join the chat at https://gitter.im/Landoop/support](https://img.shields.io/gitter/room/nwjs/nw.js.svg?maxAge=2592000)](https://gitter.im/Landoop/support?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

**Kafka UI** for Kafka data. Can browse any topic and help you understand what's happening on your cluster

> [Demo of kafka-topics](https://kafka-topics-ui.landoop.com)

**Capabilities** find topics / view topic metadata / browse topic data (kafka messages) / view topic configuration / download data

Currently uses the [confluentinc/kafka-rest proxy](https://github.com/confluentinc/kafka-rest), but future versions will
be capable of interacting with topics via other means as well.

### Other interesting projects

|                                                                       | Description                                                                  |
|-----------------------------------------------------------------------| -----------------------------------------------------------------------------|
| [schema-registry-ui](https://github.com/Landoop/schema-registry-ui)   | View, create, evolve and manage your **Avro Schemas** on your Kafka cluster  | 
| [Landoop/fast-data-dev](https://github.com/Landoop/fast-data-dev)     | Docker for Kafka developers (schema-registry,kafka-rest,zoo,brokers,landoop) |
| [Landoop-On-Cloudera](https://github.com/Landoop/Landoop-On-Cloudera) | Install and manage your kafka streaming-platform on you Cloudera CDH cluster |

## Preview

<a href="http://kafka-topics-ui.landoop.com" target="_blank" width="50%">
    <img src="https://raw.githubusercontent.com/Landoop/kafka-topics-ui/gh-pages/v0.7.1-topics-1.png">
</a>

<a href="http://kafka-topics-ui.landoop.com" target="_blank" width="50%">
    <img src="https://raw.githubusercontent.com/Landoop/kafka-topics-ui/gh-pages/v0.7.1-topics-2.png">
</a>

## Running it

To run it standalone through Docker

    docker pull landoop/kafka-topics-ui
    docker run --rm -it -p 8000:8000 \
               -e "SCHEMAREGISTRY_UI_URL=http://confluent-schema-registry-host:port" \
               -e "KAFKA_REST_PROXY_URL=http://kafka-rest-proxy-host:port" \
               landoop/kafka-topics-ui

Note that the schema registry is optional. If not provided, topics will attempt to be parsed using Avro, then fall back to JSON, and finally fall back to Binary if both of those fail.

**Config:** `Kafka-Rest-Proxy` CORS is a bit buggy at the latest release, so we will need to
provide CORS through a proxy (i.e. nginx)

Example for nginx

    location / {
      add_header 'Access-Control-Allow-Origin' "$http_origin" always;
      add_header 'Access-Control-Allow-Credentials' 'true' always;
      add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
      add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Mx-ReqToken,X-Requested-With' always;

      proxy_pass http://kafka-rest-server-url:8082;
      proxy_redirect off;

      proxy_set_header  X-Real-IP  $remote_addr;
      proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header  Host $http_host;
    }

> We also provide the kafka-topics-ui as part of the [fast-data-dev](https://github.com/Landoop/fast-data-dev), that
is an excellent docker for developers

### Building it

* You need to download dependencies with `bower`. Find out more [here](http://bower.io)
* You need a `web server` to serve the app.
* By default `kafka-topics-ui` points to some default locations like `http://localhost:8081`
  To point it to the correct backend servers, update `src/env.js`

### Steps

    git clone https://github.com/Landoop/kafka-topics-ui.git
    cd kafka-topics-ui
    npm install
    http-server .

Web UI will be available at `http://localhost:8080`

### Nginx config

If you use `nginx` to serve this ui, let angular manage routing with

    location / {
        try_files $uri $uri/ /index.html =404;
        root /folder-with-kafka-topics-ui/;
    }

## License

The project is licensed under the [BSL](http://landoop.com/bsl) license
