# Laboratório do Capítulo 13 - Cluster ElasticSearch
Nesse capítulo vamos realizar a instalaçao de um cluster Elasticsearch, no meu exemplo vou usar o Google GCP, mas você pode fazer com, maquinas virtuais, Amazon, Azure, como achar melhor.
Tanto a Amazon, como Azure e GCP disponibilizam de um periodo de testes grátis onde você pode criar a sua infra sem custo, sendo assim basta criar uma conta em um desses provedores cloud e criar sua infraestrutura.

## Criando sua infra no Google GCP:
Nesse vídeo eu explico como fazer a criação dos 3 servidores CentOS que serão usados na configuração do cluster. Serão criados 3 servidores com 4GB de RAM 2 CPUs e 50GB de disco cada um.
Os nomes dos servidores serão:
- elliot01
- elliot02
- elliot03

## Hosts Cluster Elasticsearch:
10.128.0.7      elliot01    elliot01.anselmo.com
10.128.0.6      elliot02    elliot02.anselmo.com
10.128.0.5      elliot03    elliot03.anselmo.com

## Criando Alias para cada um dos nodes
alias elliot01="ssh Anselmo@elliot01"
alias elliot02="ssh Anselmo@elliot02"
alias elliot03="ssh Anselmo@elliot03"

## Instalando o OpenJDK nos 3 nodes:
elliot01 "sudo yum install java-1.8.0-openjdk.x86_64 -y"
elliot02 "sudo yum install java-1.8.0-openjdk.x86_64 -y"
elliot03 "sudo yum install java-1.8.0-openjdk.x86_64 -y"

## Alteraçao do hostname:
Alterar o nome dos hosts nos 3 nodes:

### Elliot01
elliot01 "sudo hostnamectl set-hostname elliot01.anselmo.com"

### Elliot02
elliot02 "sudo hostnamectl set-hostname elliot02.anselmo.com"

### Elliot03
elliot03 "sudo hostnamectl set-hostname elliot03.anselmo.com"

## Copiando o repositório do Elasticseach para os nodes:
Colar o conteudo abaixo no caminho /etc/yum.repos.d criando um arquivo de nome elasticsearc.repo em cada um dos nodes:

[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

### Elliot01
elliot01 sudo vi /etc/yum.repo.d/elasticsearch.repo
(cole o conteúdo e salve)

### Elliot02
elliot02 sudo vi /etc/yum.repo.d/elasticsearch.repo
(cole o conteúdo e salve)

### Elliot03
elliot03 sudo vi /etc/yum.repo.d/elasticsearch.repo
(cole o conteúdo e salve)
