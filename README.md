# Worker

**Worker** is a simple HTTP server that accepts FQL queries, executes them and returns their results.
OpenAPI v2 schema can be found [here](https://github.com/molly1022/Web-Scraping/blob/main/reference/ferret-worker.yaml).

## Quick start

The Worker is shipped with dedicated Docker image that contains headless Google Chrome, so feel free to run queries using `cdp` driver:

DockerHub
```sh
docker run -d -p 8080:8080 montferret/worker
```
GitHub
```sh
docker run -d -p 8080:8080 ghcr.io/montferret/worker
```

Alternatively, if you want to use your own version of Chrome, you can run the Worker locally.

By installing the binary:

```shell
curl https://raw.githubusercontent.com/MontFerret/worker/master/install.sh | sh
worker
```

Or by building locally:

```sh
make
```

And then just make a POST request:

![worker](https://github.com/molly1022/Web-Scraping/blob/main/assets/postman.png)

## System Resource Requirements
- 2 CPU
- 2 Gb of RAM

## Usage

### Endpoints

#### POST /
Executes a given query. The payload must have the following shape:

```
Query {
    text: String!
    params: Map<string, any>
}
```

#### GET /info
Returns a worker information that contains details about Chrome, Ferret and itself. Has the following shape:

```
Info {
    ip: String!
    version: Version! {
        worker: String!
        chrome: ChromeVersion! {
            browser: String!
            protocol: String!
            v8: String!
            webkit: String!
        }
        ferret: String!
    }
}
```


#### GET /health
Health check endpoint (for Kubernetes, e.g.). Returns empty 200.

### Run commands

```bash
  -log-level="debug"
    log level
  -port=8080
    port to listen
  -body-limit=1000
    maximum size of request body in kb. 0 means no limit.
  -request-limit=20
    amount of requests per second for each IP. 0 means no limit.
  -request-limit-time-window=180
    amount of seconds for request rate limit time window.
  -cache-size=100
    amount of cached queries. 0 means no caching.
  -chrome-ip="127.0.0.1"
    Google Chrome remote IP address
  -chrome-port=9222
    Google Chrome remote debugging port
  -no-chrome=false
    disable Chrome driver
  -version=false
    show version
  -help=false
    show this list
```
