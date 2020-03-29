# monitor-discovery

Ferramentas de monitoramento de containers e serviços

Usando
- Mikrotik
- Docker
- Node export
- Consul
- Prometheus
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

## Iniciar o Node do Mikrotik
```
# ./mikrotik-exporter -address 192.168.0.10 -device rot1 -password 12345678 -user prometheus
```
Onde -address é o endereço do seu mikrotik. -device é o nome do rótulo do dispositivo na saída de métricas para o prometheus. O usuário e senha deve ser criadas no seu mikrotik

## Adicionando Node ao Consul
```
# vi /conf/consul/node/node-mk-01.json
{
    "name": "mk 01",
    "address": "O IP onde está rodando o mikrotik-exporter",
    "port": 9436
}

Mikrotik 01
# http PUT http://localhost:8500/v1/agent/service/register < ./conf/consul/node/node-mk-01.json
```