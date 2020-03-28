# monitor-discovery

Ferramentas de monitoramento de containers e serviços

Usando
- Docker
- Prometheus
- Node exports
- Grafana

# CMD

## Instale o Docker e crie o cluster Swarm
```
# curl -fsSL https://get.docker.com | sh
# docker swarm init
```
## Clone o repositório
```
# git clone https://github.com/madson7/monitor-discovery.git

# cd monitor-discovery
```

## Build de imagen do promotheus
```
# docker build -t madson7/prometheus_alpine ./dockerfiles/prometheus
```

## Deploy Stack com Docker Swarm
```
# docker stack deploy -c docker-compose.yml discovery
```
## Status dos serviços
```
# docker service ls
```

## Adicionando Node ao Consul
```
http PUT http://localhost:8500/v1/agent/service/register < ./conf/consul/node/node-exporter-1.json
```