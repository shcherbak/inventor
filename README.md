# inventor

## Description

The Inventor is a Prometheus HTTP SD Server allows users to dynamcially add or remove prometheus targets and labels and expose it to a [Prometheus HTTP SD](https://prometheus.io/docs/prometheus/latest/http_sd/) job.

## Usage

Running the server:
```
cd src
go run main.go
```

Registering new target:
```bash
curl -X PUT -H "x-api-token: secret" http://127.0.0.1:9101/target \
-d '{"static_config": {"targets": ["10.0.10.2:9100",], "labels": {"__meta_datacenter": "dc-01", "__meta_prometheus_job": "node"}}}'
```

More examples: `./test/end-to-end`


## Configuration Environmet Valiables

  * `REDIS_ADDR`: redis server addres to store metrics and targets
  * `REDIS_PORT`: redis server port
  * `REDIS_DBNO`: redis server keyspace
  * `TTL_SECONDS`: ttl for storing target, default is 6h (21600 seconds)
  * `API_TOKEN`: API token for manipulating targets

## API Methods

* **GET /discover**
    * Returning the list of targets in Prometheus HTTP SD format
* **PUT /target**
    * Adds the new target
* **GET /target**
    * Returning target by ID
* **DELETE /target**
    * Removing target by ID
* **GET /metrics**
    * Metrics in prometheus format
* **GET /healthcheck**
    * Health Check for kubernetes deployments


## Build Docker image
```bash
docker build -t $(cat VERSION.txt) --build-arg BUILD_VERSION=$(cat VERSION.txt) -f docker/Dockerfile .
```
pulling image:
```bash
jushcherbak/inventor:0.0.1
```


## License

Covered under the [MIT license](LICENSE.md).

