# Monitor-Discovery

![](img/schema.png)

Ferramentas de monitoramento de hosts, containers e serviços

- Node Exporter's
- Docker
- Cadvisor
- Consul
- Prometheus
- Grafana

Infraestrutura

- Mikrotik
- Server Linux

Instalação do Docker e iniciando o cluster Swarm
```
# curl -fsSL https://get.docker.com | sh
# docker swarm init
```

Clonado o repositório monitor-discovery
```
# git clone https://github.com/madson7/monitor-discovery.git

# cd monitor-discovery
```

Build de imagen do Promotheus
```
# docker build -t madson7/prometheus_alpine ./dockerfiles/prometheus
```

Deploy Stack com Docker Swarm
```
# docker stack deploy -c docker-compose.yml discovery
```

Status dos serviços
```
# docker service ls
```

Iniciar o Node do Mikrotik
```
# ./conf/node/mikrotik-exporter -address 192.168.0.10 -device rot1 -password 12345678 -user prometheus
```
Onde -address é o endereço do seu mikrotik. -device é o nome do rótulo do dispositivo na saída de métricas para o prometheus. O usuário e senha deve ser criadas no seu mikrotik

Iniciar o Node do Linux
```
# ./conf/node/node_exporter --web.listen-address=":9101"
```
Onde --web.listen-address=":9101" é a porta do seu servidor Linux local.

Adicionando Node ao Consul
conf/consul/*.json
```
{
    "name": "Nome do serviço",
    "address": "O IP onde está rodando",
    "port": Pota de serviço
}

Mikrotik 01
# http PUT http://localhost:8500/v1/agent/service/register < ./conf/consul/mk-01.json

Cadvisor 01
# http PUT http://localhost:8500/v1/agent/service/register < ./conf/consul/cadvisor-01.json

Netedata 01
# http PUT http://localhost:8500/v1/agent/service/register < ./conf/consul/linux-01.json
```