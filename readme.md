# Elastic Stack (ELK) no Docker

Projeto baseado no setup para Docker disponível em https://github.com/deviantony/docker-elk#readme

## Executando

1. [Antes de executar](#antes-de-executar)
1. [Execução](#execução)
    * [Acessando o Kibana](#acessando-o-kibana)
1. [O arquivo Filebeat.yml](#o-arquivo-filebeat.yml)
1. [Criar visualização de dados no Kibana](#criar-visualização-de-dados-no-Kibana)
1. [Observabilidade com Elastic APM](#observabilidade-com-elastic-apm)
    * [Elastic APM + Java](elastic-apm--java)

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
* **OBSERVAÇÃO**: A configuração do arquivo de log deve seguir o modelo em json seguido pelo Logstash para que os dados coletados sejam exibidos corretamente no Kibana

## Criar visualização de dados no Kibana
* Para criar a visualização de dados no Kibana é necessário acessar o menu, buscar e seguir os cliques nas opções Management > Stack Management > Kibana > Data Views
* Dentro de Data View, clicar no botão "Create data view"
* No campo "Name" definir o nome do Data View, por padrão ele deve iniciar com os mesmo nome do índice informado no arquivo filebeat.yml, no nosso exemplo, o nome será "produtos". Depois clicar no botão "Create data view" para confirmar a criação do Data View.
* Depois é só acessar no Menuo a opção de " Analytics > Discover" para visualizar os dados coletados apartir do Filebeat.

## Observabilidade com Elastic APM
* Para ter uma aplicação Java monitorada pelo Elastic APM, além de subir o Elastic APM Server é necessário configurar um agente para coleta de métricas, o agente pode ser utilizado de forma externa a aplicação, ou como biblioteca na própria aplicação, ou ainda acionando as APIs do APM Server.
* Documentação de observabilidade com Elastic Stack - https://www.elastic.co/guide/en/observability/current/index.html
* Documentação dos agentes de APM - https://www.elastic.co/guide/en/apm/agent/index.html

### Elastic APM + Java
* Download do Elastic APM Agente externo para Java - https://search.maven.org/search?q=g:co.elastic.apm%20AND%20a:elastic-apm-agent
* Para executar o agente junto com a aplicação execute o seguinte código: ```-javaagent:<CAMINHO_ELASTIC_APM_AGENT>elastic-apm-agent-<VERSAO>.jar -Delastic.apm.service_name=<NOME_SERVICO> -Delastic.apm.application_packages=<NOME_PACOTE>,<NOME_OUTRO_PACOTE>,...<NOMES_DEMAIS_PACOTES> -Delastic.apm.server_url=<URL_PORTA_APM_SERVER> -jar <ARQUIVO_APLICACAO.jar>```, exemplo:
```
-javaagent:d:/produtos/recursos/elastic-apm-agent-1.32.0 
-Delastic.apm.service_name=produtos 
-Delastic.apm.application_packages=br.com.produtos 
-Delastic.apm.server_url=http://localhost:8200
-jar produtos-1.0.0.jar
```
**onde:**
- ```d:/produtos/recursos/elastic-apm-agent-1.32.0``` é o caminho onde encontra-se a biblioteca do agente
- ```produtos ``` é o nome do serviço/aplicação e é esse nome que será exibido no kibana para visualização das estatistícas
- ```br.com.produtos``` é o nome do pacote onde serão coletadas métricas, é possível especificar mais pacotes separando-os por "," (vírgula)
- ```http://localhost:8200``` é a url (e porta) do APM Server 
- ```produtos-1.0.0.jar``` é arquivo contendo a aplicação compilada/empacotada para execução

**No eclipse é possível executar o agente Java na aba de execução e informar o seguinte comando**
```
-javaagent:d:/produtos/recursos/elastic-apm-agent-1.32.0 
-Delastic.apm.service_name=produtos 
-Delastic.apm.application_packages=br.com.produtos 
-Delastic.apm.server_url=http://localhost:8200
```

* Para que os dados sejam exibidos no Kibana é necessário ir até a sessão  "Management > Integrations > Elastic APM > Elastic APM in Fleet > APM Integration > Add Elastic APM" e adicionar uma integração para o APM funcionar e passar a exibir as métricas da aplicação
* Depois configurar integração com, configurações de integração, configurações do APM Server, configurações de TLS, ..., criar politícas do agente (agent policy)

* Após tudo configurado, pode-se verificar as métricas em "Observability > APM > Services"