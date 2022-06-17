# Coletor de dados do Docker utilizando Docker Engine API (v1.41)     

## Descrição

Nesse tutorial, criaremos uma prova de conceito fim a fim do coletor de dados do Docker no linux. Para isso, investigamos o funcionamento da API do Docker; verificamos quais as métricas poderiam ser coletadas; subimos um container para coleta de métricas (Node-Red); subimos um container de banco de dados (InfluxDB); conectamos o coletor de métricas, o banco de dados e as chamadas de API a partir do Node-Red.

## Requisitos do sistema

* Ubuntu 18.04.6 LTS
* Pelo menos 4 GB de RAM

## Instalar o Docker e subir os containers necessários

### Instalar e atualizar o Docker
Cole o comando abaixo na CLI:
```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
### Criar uma rede
Criar uma rede de tipo overlay para comunicação entre as ferramentas.
```bash
    docker network create -d overlay --attachable coleta
```

### Subir o container do Node-Red
Cole o comando abaixo na CLI:
```bash
    docker run -it -d -p 1880:1880 -v node_red_data:/data --network coleta --name mynodered nodered/node-red 
```

### Subir o container do InfluxDB

```bash
    docker run -it -d -p 8086:8086 -v /data:/var/lib/influxdb --network coleta --name influx influxdb:latest
```
## Trabalhando com Node-Red
Para acessar a ferramenta, cole no navegador: `http://{host-ip}:1880` (ex: http://localhost:1880) e poderá começar a trabalhar.

    IMAGEM DA AREA DE TRABALHO

Agora, iremos instalar os nós necessários para execução. No canto superior a direita, vá ao menu de sanduíche -> manage palette -> install e instale os pacotes `node-red-contrib-influxdb` e `node-red-contrib-loop`.

    IMAGEM DO MENU COM OS PACOTES INSTALADOS.

Para agilizar, iremos importar os flows já prontos a partir dos JSON abaixo:

    COLAR O JSON CORRETO

    EXPLICAR O QUE ESTÁ OCORRENDO EM CADA NÓ DO FLOW E A INTEGRAÇÃO ENTRE NODERED, INFLUXDB E API DO DOCKER

