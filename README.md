# Example Docker Compose project for Nestjs, Elastic, Mongo, Telegraf, InfluxDB V2 and Grafana,

This repo create TIG stack example based on following architecture
![Architecture](./docs/architecture.png?raw=true "Architecture")

## Start the stack with docker compose

```bash
$ docker-compose up
```

## Services and Ports
Services are hidden behind nginx and not discoverable by default

### Grafana
- URL: http://localhost/grafana
- User: admin 
- Password: admin 

### Nestjs
On start nestjs will fetch list of pokemons from pokeapi.co

To get list of Pokémons(LIMIT=10). On each request we are getting data from (1) mongo (2) elastic 
- URL: http://localhost/pokemon

## Load test
[LoadTestingDockerfile](./SiegeDockerfile) contains docker file for generating load testing.
Thought there's a problem that locally you won't be able to run it via `127.0.0.1` or `localhost` because it will load test itself.
To mitigate this problem you need to run docker container and connect to web-app network, e.g.
1. Create docker container
```bash
 cd siege
 docker build -t siege-container .  
```
1. Get nginx IP (by default name is `tig_stack_nginx_1`)
```bash
  docker run --network=tig_stack_webapp-network --rm siege-container http://nginx/pokemon
```

By default, we're using:
- t=5M - It will be running for 5 minutes
- c=10000 - Number of concurrency
- b - This directive tells siege to go into benchmark mode. This means there is no delay between iterations.

Used config: [siege.config](./siege/Dockerfile)

For more options see [Siege docs](https://github.com/JoeDog/siege/blob/master/doc/siege.pod)

## Grafana dashboard
There are 3 provisioned boards in grafana
1. Requests per second
1. Response time
1. Success/Failed requests per database (mongo/elastic.)

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.

