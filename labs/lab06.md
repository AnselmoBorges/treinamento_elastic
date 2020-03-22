# Laboratório do Capítulo 13 - Cluster ElasticSearch
Nesse capítulo vamos realizar a instalaçao de um cluster Elasticsearch, no meu exemplo vou usar o Google GCP, mas você pode fazer com, maquinas virtuais, Amazon, Azure, como achar melhor.
Tanto a Amazon, como Azure e GCP disponibilizam de um periodo de testes grátis onde você pode criar a sua infra sem custo, sendo assim basta criar uma conta em um desses provedores cloud e criar sua infraestrutura.

## Criando sua infra no Google GCP:
Nesse vídeo eu explico como fazer a criação dos 3 servidores CentOS que serão usados na configuração do cluster. Serão criados 3 servidores com 4GB de RAM 2 CPUs e 50GB de disco cada um.
Os nomes dos servidores serão:
- elliot01
- elliot02
- elliot03

## Video criaçao Infra GCP
[Criando infra GCP](https://drive.google.com/file/d/1eVMvi4nLvAdMr5QKbedZps6fOIekk_oL/view?usp=sharing)

*OBS: Todo esse material foi montando partindo do principio que a maquina origem (a minha) é um linux ou um Mac, para Windows algumas coisas deverão ser adaptadas. E eu estou falando da estação de trabalho não dos servidores, o dia que eu instalar um Cluster no Windows eu paro de trabalhar*

## Adicionando no host da sua maquina o hosts desses servidores criados:
No /etc/hosts da minha instação adiciono os hosts das maquinas com os IPs externos dela, conforme o video orienta, substitua esses pelo de vocês:

    # Hosts servidores do Cluster:
    34.68.243.71            elliot01                elliot01.anselmo.com
    34.71.126.7             elliot02                elliot02.anselmo.com
    35.193.136.17           elliot03                elliot03.anselmo.com

## Criando Alias para cada um dos nodes
Criei alias para facilitar na digitação dos comando, faça caso você ache necessário, eu acho que me ajuda muito:

    alias elliot01="ssh Anselmo@elliot01"
    alias elliot02="ssh Anselmo@elliot02"
    alias elliot03="ssh Anselmo@elliot03"

## Hosts Cluster Elasticsearch:
Acrescentar no arquivo /etc/hosts de cada um dos servidores o conteudo abaixo, substitua o conteudo pelos seus IPs privados:

    10.128.0.7      elliot01    elliot01.anselmo.com
    10.128.0.6      elliot02    elliot02.anselmo.com
    10.128.0.5      elliot03    elliot03.anselmo.com

*Elliot01*
    
    elliot01
    vim /etc/hosts
    (cole o conteúdo, salve e saia)

*Elliot02*
    
    elliot02
    vim /etc/hosts
    (cole o conteúdo, salve e saia)

*Elliot03*
    
    elliot03
    vim /etc/hosts
    (cole o conteúdo, salve e saia)

## Alteraçao do hostname:
Alterar o nome dos hosts nos 3 nodes para que fique facil a identificação dos nodes:

    elliot01 "sudo hostnamectl set-hostname elliot01.anselmo.com"
    elliot02 "sudo hostnamectl set-hostname elliot02.anselmo.com"
    elliot03 "sudo hostnamectl set-hostname elliot03.anselmo.com"

## Instalando o OpenJDK nos 3 nodes:
Como já dito anteriormente o Java 8 é pré requisito para o funcionamento do Elasticsearch, vamos fazer a instalação nos 3 nodes.

    elliot01 "sudo yum install java-1.8.0-openjdk.x86_64 -y"
    elliot02 "sudo yum install java-1.8.0-openjdk.x86_64 -y"
    elliot03 "sudo yum install java-1.8.0-openjdk.x86_64 -y"

## Configurando o repositório do Elasticseach para os nodes:
Colar o conteudo abaixo no caminho /etc/yum.repos.d criando um arquivo de nome elasticsearch.repo em cada um dos nodes:

    [elasticsearch]
    name=Elasticsearch repository for 7.x packages
    baseurl=https://artifacts.elastic.co/packages/7.x/yum
    gpgcheck=1
    gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
    enabled=1
    autorefresh=1
    type=rpm-md

Sendo assim entre nos servidores e faça a edição:

    elliot01 
    sudo vi /etc/yum.repos.d/elasticsearch.repo
    (cole o conteúdo e salve)
    exit

    elliot02 
    sudo vi /etc/yum.repos.d/elasticsearch.repo
    (cole o conteúdo e salve)
    exit

    elliot03 
    sudo vi /etc/yum.repos.d/elasticsearch.repo
    (cole o conteúdo e salve)
    exit

## Instalação dos pacotes RPM:
Agora com os repositórios configurados vamos realizar a instalação dos pacotes do Elasticsearch nos 3 nodes:

    elliot01 "sudo yum install elasticsearch -y"
    elliot02 "sudo yum install elasticsearch -y"
    elliot03 "sudo yum install elasticsearch -y"

## Configuração do Kibana:
Somente no node 1 nós vamos configurar o repositorio do Kibana no /etc/yum.repos.d criando um arquivo kibana.repo colando o conteúdo abaixo:

    [kibana-7.x]
    name=Kibana repository for 7.x packages
    baseurl=https://artifacts.elastic.co/packages/7.x/yum
    gpgcheck=1
    gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
    enabled=1
    autorefresh=1
    type=rpm-md

Cole o conteudo:

    elliot01 
    sudo vi /etc/yum.repos.d/kibana.repo
    (cole o conteúdo e salve)
    exit

## Instale o Kibana:
Agora com o repositório configurado instalamos o Kibana somente no primeiro node:

    elliot01 "sudo yum install kibana -y"
    
## Configuração do parâmetros do Cluster e finalização:
Agora que temos o tudo instalado, vamos configurar os parâmetros pra que os nodes se comuniquem e virem um cluster.
