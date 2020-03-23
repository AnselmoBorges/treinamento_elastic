# Lab 7 - Habilitando a autenticaçao no elasticsearch e a criptografia TLS
Nesse Lab vamos fazer a habilitação da segurança no cluster Elasticsearch e no Kibana, criando uma autenticação inicial e habilitando certificados TLS na comunicação entre os nodes.

## Gerando as chaves TLS/SSL
Para gerar as chaves TLS rodamos o comando abaixo, rodamos no node 1 primeiro:
  
  elasticsearch-certutil cert -out /etc/elasticsearch/elastic-certificates.p12 -pass ""

## Habilitando o Xpack no arquivo de configuração
Para habilitar a autenticação temos que habilitar o xpack.security nos 3 nodes no /etc/elasticsearch/elasticsearch.yml. Cole o conteudo abaixo nos 3 nodes, note que ele já está considerando a criação do certificado .p12 criado no tópico anterior.

  xpack.security.enabled: true
  xpack.security.transport.ssl.enabled: true
  xpack.security.transport.ssl.verification_mode: certificate
  xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
  xpack.security.transport.ssl.truststore.path: elastic-certificates.p12

## Copie o certificado para os outros nodes:
Agora como criamos o certificado somente no primeio node, vamos copiar ele para os outros nodes no diretório /etc/elasticsearch

## Reinicie o Elasticsearch em todos os nodes:
Com tudo feito nos reiniciamos o cluster nos 3 nodes

  systemctl restart elasticsearch

## Rodando o utilitário de criação de password:
Agora vamos rodar o utilitário que cria os usuários para autenticação, no node 1 rode o comando abaixo (após executado no node 1, não é necessário fazer nos demais nodes).

  elasticsearch-setup-passwords interactive

## Criando um keystore para o Kibana
Para que a senha não fique exposta, vamos criar um keystore para o kibana se conectar com o Elastic

  kibana-keystore create

Agora vamos adicionar o usuário e senha nesse keystore

  kibana-keystore add elasticsearch.username
  kibana-keystore add elasticsearch.password
  
## Restart do Kibana e tentativa de login
Faça o restart do Kibana com o comando abaixo e posteriormente faça o teste de login via navegador utilizando o usuário "elastic" e a senha que você definiu.
