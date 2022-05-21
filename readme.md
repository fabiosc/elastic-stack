# Elastic Stack (ELK) no Docker

Projeto baseado no setup para Docker disponível em https://github.com/deviantony/docker-elk#readme

## Executando

1. [Antes de executar](#antes-de-executar)
1. [Execução](#execução)
    * [Acessando o Kibana](#acessando-o-kibana)
1. [O arquivo Filebeat.yml](#o-arquivo-filebeat.yml)
1. [Criar visualização de dados no Kibana](#criar-visualização-de-dados-no-Kibana)

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
* URL onde os logs da aplicação serão coletados para serem repassados ao Filebeat e uso na Elastic Stack.

## Execução

No diretório (pasta) onde o arquivo docker-compose.yml encontra-se executar o comando: 

### docker-compose up -d

Após concluída toda a execução, a stack dever desligada com o comando docker-compose down e posteriormente reexecutar o comando docker_compose up -d.

## Acessando o Kibana
Para acessar o Kibana, basta acessar no browser a URL http://localhost:5601, o usuário de acesso, é "elastic" e a senha a que foi definida na variável de ambiente "ELASTIC_PASSWORD"

## O arquivo filebeat.yml
* O arquivo filebeat.yml é o responsável por configurar a sincronização entre dos dados de log da aplicação e a Elastic Stack.
* Algumas configurações são relativas ao nome do índice no Elastic Search e seu template. Nesse exempo o nome do índice tem nome iniciado pela palavra "produtos". E pode ser alterado para o nome que achar mais conveniente para criação dos índice e seu template.
* OBSERVAÇÃO: A configuração do arquivo de log deve seguir o modelo em json seguido pelo Logstash para que os dados coletados sejam exibidos corretamente no Kibana

## Criar visualização de dados no Kibana
* Para criar a visualização de dados no Kibana é necessário acessar o menu, buscar e seguir os cliques nas opções Management > Stack Management > Kibana > Data Views
* Dentro de Data View, clicar no botão "Create data view"
* No campo "Name" definir o nome do Data View, por padrão ele deve iniciar com os mesmo nome do índice informado no arquivo filebeat.yml, no nosso exemplo, o nome será "produtos". Depois clicar no botão "Create data view" para confirmar a criação do Data View.
* Depois é só acessar no Menuo a opção de " Analytics > Discover" para visualizar os dados coletados apartir do Filebeat.
