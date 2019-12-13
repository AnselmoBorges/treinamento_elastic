# Laboratório 3 - Realizando Consultas
**Criado por:** Anselmo Borges
**Ultima Atualização:** 13.12.2019

Nesse laboratório iremos realizar algumas consultas no schema adega criado no lab anterior:

## Consultando no index adega todos os documentos:
> GET /adega/_search

## Consultando somente as cervejas:
> GET /adega/_search
{
  "query": {
    "match" : {
      "bebida": "Cerveja"
    }
  }
}

## Consultando somente os produtos de Lata Independente da quantidade em ml:
> GET /adega/_search
{
  "query": {
    "match" : {
      "tipo": "Lata"
    }
  }
}

## Consultando somente os produtos de Garrafa Independente da quantidade em ml:
> GET /adega/_search
{
  "query": {
    "match" : {
      "tipo": "Garrafa"
    }
  }
}

## Contando o numero de documentos:
> GET /adega/_count

## Realizando a instalação e ingestão dos dados do blogs.csv
O arquivo blogs.csv encontra-se aqui no git na pasta datasets e o arquivo de configuração do logstash encontra-se no diretorio parâmetros. Siga os passos abaixo para poder realizar o procedimento:

* Baixe o Logstash com o comando abaixo
   * wget https://artifacts.elastic.co/downloads/logstash/logstash-7.5.0.tar.gz
* Realize a descompactação do arquivo baixado com o comando abaixo:
   * tar zfxv logstash-7.5.0.tar.gz
* Altere no arquivo blogs_csv_lab3.conf o caminho de onde está o arquivo blogs.csv, deve ser o caminho completo.
* Execute o start do logstash e use o parametro -f para o .conf, conforme exemplo abaixo:
   * /caminho/do/logstash/logstash-7.5.0/bin/logstash -f /caminho/do/conf/blogs_csv_lab3.conf
* Se necessário altere o endereço do elasticsearch pra onde está sendo realizada a importação, no meu caso estou usando o localhost, a alteração deve ser feita no arquivo blogs_csv_lab3.conf, na área output.

## Realizando o count do numero de registros importados pelo Logstash no indice blogs (tem que ter 1594 documentos):
> GET /blogs/_count

## Realizando a importação dos logs temporais via Filebeat:
Vamos realizar a instalação do filebeat do fonte e usar o arquivo de configuração editado para enviar pro Elasticsearch os dados. Os passos para executar essa tarefa:
* Clone esse repositório.
* Baixe o Filebeat com o comando abaixo
   * wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.5.0-linux-x86_64.tar.gz
* Realize a descompactação do arquivo baixado com o comando abaixo:
   * tar zfxv filebeat-7.5.0-linux-x86_64.tar.gz
* Altere no arquivo filebeat.yml o caminho de onde está o arquivo de logs elastic_blog_curated_access_logs.tar.gz, deve ser o caminho completo.
* Extraia o conteúdo de elastic_blog_curated_access_logs.tar.gz
* Execute o start do filebeat e use o parametro -c para o filebeat.yml, conforme exemplo abaixo:
   * /caminho/do/logstash/filebeat-7.5.0/filebeat -c /caminho/do/conf/filebeat.yml
* Se necessário altere o endereço do elasticsearch pra onde está sendo realizada a importação, no meu caso estou usando o localhost, a alteração deve ser feita no arquivo filebeat.yml, na área output.

A importação dos logs vai ser feita para o Elasticsearch criando 3 indices:
* log_server1
* log_server2
* log_server3

## Validando a quantidade de logs importados.
Para acompanhar o andamento da importação dos logs use o comando abaixo:
> GET logs_server*/_count

## Adicionando os indices para vizualização no Kibana:
Ná área de Management do Kibana vamos clicar em index patterns:<br>

![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/management_index.png)

Feito isso clicamos no Icone azul de Create Index Patterns:<br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/create_index_patterns.png)

Escreva logs_server*, aparecerão os 3 indices criados pelo filebeats, clique em next step.<br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/include_index.png)

Em Time filter field, escolha @timestamp e clique em create:
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/time_field.png)

## Criando uma visualização desses indices no Kibana:
Vamos agora criar uma visualização simples desses logs no Kibana, clicamos na barra lateral Visualize eclicamos em create new visualization.<br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/visualize.png)

Vamos escolher o tipo Data Table de visualização:<br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/datatable.png)

Escolheremos como datasource log_server*:<br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/datasource.png)

Agora na tela de construção da visualização vamos em Buckets, Split Rows, em agregação vamos pra Terms, em fields vamos em originalUrl.keyword, em Custom Label coloque "5 Posts mais acessados do Blog" e clicamos no Play la em cima:<br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/visu_index.png)

Após o play não vai ser exibido nada, isso é porque os dados são de 3 anos atras, modifique o filtro para last 3 years (na imagem está 2 mais coloque 3): <br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/filter.png)

Agora Salvamos a visualização:<br>
![Index Patterns](https://github.com/AnselmoBorges/treinamento_elastic/blob/master/imagens/save.png)
