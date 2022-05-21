# Elastic Stack (ELK) no Docker

Projeto baseado no setup para Docker disponível em https://github.com/deviantony/docker-elk#readme

## Executando

1. [Antes de executar](#prerequisitos)
1. [Execução](#execucao)
    * [Acessando o Kibana](#acessando-kibana)

## Antes de executar
O arquivo .env contém as variáveis responsáveis por padronizar a instalação da Elastic Stack, são elas:

### ELASTIC_VERSION
* Possui a configuração da versão da Elastic Stack a ser instalada

### ELASTIC_PASSOWRD
* Senha do usuário "elastic" no Elastic Search

### LOGSTASH_INTERNAL_PASSWORD
* Senha do usuário "logstach_internal" no Logstash

### KIBANA_SYSTEM_PASSWORD
* Senha do usuário "kibana_system" no Kibana

### FILEBEAT_LOG_URL
* URL onde os logs da aplicação serão coletados para serem repassados ao Filebeat e uso na Elastic Stack

## Execução

No diretório (pasta) onde o arquivo docker-compose.yml encontra-se executar o comando: 

### docker-compose up -d

Após concluída toda a execução, a stack dever desligada com o comando docker-compose down e posteriormente reexecutar o comando docker_compose up -d.


## Acessando o Kibana
Para acessar o Kibana, basta acessar no browser a URL http://localhost:5601, o usuário de acesso, é "elastic" e a senha a que foi definida na variável de ambiente "ELASTIC_PASSWORD"