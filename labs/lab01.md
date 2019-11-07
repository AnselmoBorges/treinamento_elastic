# Laboratório 1 - Instalação do ElasticSearch e Kibana a partir do fonte:
Nesse laboratório iremos realizar as seguintes instalações:
* Java 8
* Elaticsearch 7.4
* Kibana 7.4

## Instalação do Java 8:
Iremos instalar o pacote OpenJDK 8 pelo YUM com o comando abaixo:<br>
``sudo yum install java-1.8.0-openjdk.x86_64 -y`` 

Agora como já existe o Java 7 instalado por padrão vamos mudar para o java 8 como default, com o comando abaixo, escolha a opção 2 e dê enter:<br>
``sudo update-alternatives --config java``

Vamos validar a versão do Java:<br>
``java -version``

## Mudando os parametros de Hardlimt e softlimit de openfiles
Adicione as entradas abaixo no arquivo /etc/security/limits.conf<br>
``sudo vi /etc/security/limits.conf``

``ec2-user        hard nofile 65535``<br>
``ec2-user        soft nofile 65535``

**OBS: Para salvar no vi ao invés de ESC digite CTRL+C e dê :wq normalmente.**

## Mudando o parametro de open files no /etc/sysctl.conf 
Digite o comando "sudo vi /etc/sysctl.conf" e adicione a seguinte entrada no final do arquivo.<br>
``vm.max_map_count = 262144`` 

**OBS: Para salvar no vi ao invés de ESC digite CTRL+C e dê :wq normalmente.**

Efetive as mudanças rodando o comando abaixo:<br>
``sudo sysctl -p``

## Download e instalação do Elasticsearch 7.4:
Vamos agora realizar a instalação baixando o pacote tar.gz do Elasticsearch pelo wget:<br>
``wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.4.2-linux-x86_64.tar.gz`` 

Agora descompactamos o pacote:<br>
``tar zfxv elasticsearch-7.4.2-linux-x86_64.tar.gz``

Vamos fazer algumas edições no arquivo de configuração elasticsearch.yml <br>
``cd elasticsearch-7.4.2
vi config/elasticsearch.yml``

Os parametros que vamos alterar serão os seguintes (descomente tirando o "#" e coloque conforme abaixo):
* cluster.name: crs-lab
* node.name: node01
* network.host: "coloque seu IP privado"
* http.port: 9200
* discovery.seed_hosts: ["coloque seu IP prvado"]
* cluster.initial_master_nodes: ["node01"]

**OBS: Para salvar no vi ao invés de ESC digite CTRL+C e dê :wq normalmente.**

Inicie o ElasticSearch:<br>
``bin/elasticsearch``

Se deu tudo certo abra outra aba de conexão e rode o comando abaixo:<br>
``curl -X GET "seuipprivado:9200/?pretty"``

o resultado deve ser parecido com esse:<br>
``[ec2-user@ip-172-31-16-68 ~]$ curl -X GET "172.31.16.68:9200/?pretty"``<br>                                                        
``{``<br>                                                                                                                            ``"name" : "node01",``<br>                                                                                                         
  ``"cluster_name" : "crs-lab",``<br>                                                                                                
  ``"cluster_uuid" : "MOMNcDfHSuqyFE_LwQoeEg",``<br>                                                                                 
  ``"version" : {``<br>                                                                                                              
    ``"number" : "7.4.2",``<br>                                                                                                      
    ``"build_flavor" : "default",``<br>                                                                                              
    ``"build_type" : "tar",``<br>                                                                                                    
    ``"build_hash" : "2f90bbf7b93631e52bafb59b3b049cb44ec25e96",``<br>                                                               
    ``"build_date" : "2019-10-28T20:40:44.881551Z",``<br>                                                                            
    ``"build_snapshot" : false,``<br>                                                                                                
    ``"lucene_version" : "8.2.0",``<br>                                                                                              
    ``"minimum_wire_compatibility_version" : "6.8.0",``<br>                                                                          
    ``"minimum_index_compatibility_version" : "6.0.0-beta1"``<br>                                                                    
  ``},``<br>
  ``"tagline" : "You Know, for Search"``<br>                                                                                         
``}``<br>                                  

Podemos fazer o mesmo teste via Navegador usando o endereço abaixo:<br>
``http://seupublicdns:9200``

## Instalando o Kibana:
Fazendo o download do Kibana:<br>
``cd ~
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.4.2-linux-x86_64.tar.gz``

Descompacte o Kibana:<br>
``tar zfxv kibana-7.4.2-linux-x86_64.tar.gz``

Vamos editar o arquivo de configuração do Kibana apontando para o nosso ElasticSearch:<br>
``cd kibana-7.4.2-linux-x86_64
vi config/kibana.yml``

Vamos editar os seguintes parametros:<br>
* server.host: "seupiprivado"
* server.name: "kibana-srv"
* elasticsearch.hosts: ["http://seuipprivado:9200"]

**OBS: Para salvar no vi ao invés de ESC digite CTRL+C e dê :wq normalmente.**

Iniciando os serviços do Kibana:<br>
``bin/kibana``

Teste o acesso ao Kibana via navegador no endereço abaixo:<br>
``http://seupublicdns:5601``

Entrando no Kibana rode o mesmo GET que foi rodado via CURL no devtools:<br>
``GET /``

**OBS: Para executar pressione CTRL + ENTER
