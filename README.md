# Coletor de dados do Docker utilizando Docker Engine API (v1.41)     

## Descrição

Nesse tutorial, criaremos uma prova de conceito fim a fim do coletor de dados do Docker no linux. Para isso, investigamos o funcionamento da API do Docker; verificamos quais as métricas poderiam ser coletadas; subimos um container para coleta de métricas (Node-Red); subimos um container de banco de dados (InfluxDB); conectamos o coletor de métricas, o banco de dados e as chamadas de API a partir do Node-Red. Para facilitar, iremos subir tudo via docker-compose a partir do arquivo coleta.yml.    

## Requisitos do sistema

* Ubuntu 18.04.6 LTS
* Pelo menos 4 GB de RAM

## Instalar o Docker e subir os serviços e redes via docker-compose

### Instalar e atualizar o Docker
Cole o comando abaixo na CLI:
```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
### Clonar esse repositório e executar o arquivo coleta.yml
```bash
    git clone https://github.com/lucas-schierholt/ColetorDeMetricas-DockerStats
    cd ColetorDeMetricas-DockerStats/
    chmod -R 777 nodered_data/
    docker-compose -f coleta.yml up -d
```
O arquivo yml sobe os serviços do Node-Red e do InfluxDB setando as variáveis de ambiente e pacotes necessários para realização do experimento. Além disso, é criado uma rede coleta para comunicação entre os containers.

## Trabalhando com Node-Red
O acesso a ferramente deverá ser feito pela porta 1880, cole no navegador: `http://{host-ip}:1880` (ex: http://localhost:1880). 
Terá dois flows já setados no Node-Red:
    1. 'Docker Stats': modelo base que realiza a coleta das métricas de um container Docker e armazena no banco de dados do InfluxDB.
    2. 'Create Flows from Active Container': cria flows a partir de dos containers ativos e deleta os mesmos.
<p align="center">
    <img src="/tree/main/media/images/nodered-environment.png" />
</p>
Ao clicar no nó de inject "Create Flows From Active Containers", o Node-Red acessa a API do Docker e cria os flows com o nome dos containers que estão rodando no Docker. A partir da criação, o monitoramento das métricas já é iniciado e coleta os dados para o InfluxDB. Para parar o monitoramento basta deletar os flows no nó de inject "Delete Flows".

## Trabalhando com InfluxDB
O acesso a ferramenta deverá ser feito pela porta 8086, cole no navegador: `http://{host-ip}:8086` (ex: http://localhost:1880). 
O usuário e senha do InfluxDB foram setados pelas variáveis de ambiente.
    Usuário: admin
    Senha: admin1234
No menu 'Data Explorer' acessar o Bucket 'database' -> no menu '_measurement' escolher um ou mais containers -> no menu '_field' escolher uma ou mais métricas do Docker Stats.
Ajustar o intervalo de tempo que quer monitorar -> clicar no botão 'submit' para análise do gráfico. Ou então poderá baixar o arquivo com os dados selecionados.
<p align="center">
    <img src="./tree/main/media/images/influxdb-environment.png" />
</p>

