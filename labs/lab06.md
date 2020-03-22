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
    
## Video da instalação dos pacotes:
Segue abaixo o link do vídeo com a instalação dos pacotes:

[Instalando pre-requisitos e pacotes](https://drive.google.com/file/d/1atl7CfsbcR1jDdymlRtAE6na_RQ7UIhn/view?usp=sharing)
    
## Configuração do parâmetros do Cluster e finalização:
Agora que temos o tudo instalado, vamos configurar os parâmetros pra que os nodes se comuniquem e virem um cluster de fato. Abaixo seguem os parâmetros que devemos alterar no arquivo /etc/elasticsearch/elasticsearch.yml

- **cluster.name:** Colocamos o nome do Cluster, vou chamar de crs_anselmo
- **node.name:** Colocamos o nome do node, por exemplo no node 1, elliot01
- **discovery.seed_hosts:** Aqui colocamos a lista dos hosts que farão parte do cluster, por exemplo ["server1", "server2", "server3"]
- **cluster.initial_master_nodes:** Aqui colocamos os nodes que subirão inicialmente como Master ["node1", "node2", "node3"]

**OBS: O node name é diferente do hostname, mas eu costumo configurar tudo com o mesmo nome, o node1 por exemplo, vai ter o mesmo nome do host ou seja nodename = elliot01**

Sendo assim vamos configurar dessa forma:

    cluster.name: crs_anselmo
    node.name: elliot0? #Mudar de acordo com o host
    network.host: _site_
    discovery.seed_hosts: ["elliot01", "elliot02", "elliot03"]
    cluster.initial_master_nodes: ["elliot01", "elliot02", "elliot03"]
    
Sendo assim vamos editar o arquivo em cada um dos hosts:
 
* Elliot01 *
 
    elliot01
    sudo vim /etc/elasticsearch/elasticsearch.yml
    (editar conteudo, salvar e sair)
    
* Elliot02 *
 
    elliot02
    sudo vim /etc/elasticsearch/elasticsearch.yml
    (editar conteudo, salvar e sair)

* Elliot03 *
 
    elliot03
    sudo vim /etc/elasticsearch/elasticsearch.yml
    (editar conteudo, salvar e sair)

## Habilitando o Daemon e reiniciando o serviço:
Após editado, reiniciamos o serviço nos 3 nodes:

    elliot01 "sudo systemctl enable elasticsearch; sudo systemctl restart elasticsearch"
    elliot02 "sudo systemctl enable elasticsearch; sudo systemctl restart elasticsearch"
    elliot03 "sudo systemctl enable elasticsearch; sudo systemctl restart elasticsearch"
    
## Configurando o Kibana para apontar para ElasticSearch:
No Kibana eu configuro 3 parâmetros:
- **server.host:** Coloco o host onde esse cara vai rodar, no meu caso que é o node1 será elliot01
- **server.name:** Geralmente tambem coloco o nome do host, no meu caso que é o node1 será elliot01
- **elasticsearch.hosts:** Aqui coloco o caminho para me conectar ao Elasticsearch, qualquer um dos hosts, no meu caso vou usar [http://elliot01:9200]

Então logamos no elliot01 e rodamos o comando abaixo:

    elliot01
    sudo su - 
    vim /etc/kibana/kibana.yml
    (Encontre os parâmetros, edite, salve e saia)
    
## Reiniciando e habilitando o Kibana:
Agora com o arquivo configurado, vamos reiniciar e habilitar o serviço do Kibana com o comando baixo:

    elliot01 "sudo systemctl enable kibana; sudo systemctl restart kibana"
    
## Liberando regras do firewall:
Para que você tenha acesso aos servidores da sua máquina, você vai precisar liberar o firewall nas portas 5601 (Kibana) e 9200 (Elasticsearch). O video dessa parte explica como fazer isso.

## Testando os acesso:
Vamos validar o funcionamento via navegador tentando acessar os 2 endereços abaixo:

Elasticsearch 
    
    http://elliot01.anselmo.com:9200
    
Kibana

    http://elliot01.anselmo.com:5601
    
    
## Video da configuração do Elasticsearch como cluster:
segue o link abaixo com um video explicativo da configuração do Elasticsearch como cluster.
