Por padrão o Elasticsearch não vem configurado com a autenticação. Isso pode ser uma brecha de segurança, nesse video vemos como ativar a autenticação no Cluster

## Habilitando a autenticaçao no elasticsearch e a criptografia TLS



## Gerando as chaves TLS/SSL

elasticsearch-certutil cert -out /etc/elasticsearch/elastic-certificates.p12 -pass ""



## Habilitando o Xpack no arquivo de configuração

xpack.security.enabled: true

xpack.security.transport.ssl.enabled: true

xpack.security.transport.ssl.verification_mode: certificate

xpack.security.transport.ssl.keystore.path: elastic-certificates.p12

xpack.security.transport.ssl.truststore.path: elastic-certificates.p12



## Copie o certificado para os outros nodes:

para o diretório /etc/elasticsearch



## Reinicie o Elasticsearch em todos os nodes:

systemctl restart elasticsearch



## Rodando o utilitário de criação de password:

elasticsearch-setup-passwords interactive



### Para que a senha não fique exposta, vamos criar um keystore para o kibana se conectar com o Elastic

kibana-keystore create



## Agora vamos adicionar o usuário e senha nesse keystore

kibana-keystore add elasticsearch.username

kibana-keystore add elasticsearch.password
