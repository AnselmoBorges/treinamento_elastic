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


>! PUT /adega/_doc/11
{​
   "bebida": "Wisky",​
   "marca": "Jack Daniel's",​
   "estoque": 12,​
   "valor": "109,90",​
   "tipo": "Garrafa 750ml"​
}

>! PUT /adega/_doc/12
{​
   "bebida": "Wisky",​
   "marca": "Johnny Walker Red label",​
   "estoque": 8,​
   "valor": "63,90",​
   "tipo": "Garrafa 750ml"​
}

4. Atualizar o campo preço do id 12 no modo completo para 73,30:
>! POST /adega/_doc/12
{​
   "bebida": "Wisky",​
   "marca": "Johnny Walker Red label",​
   "estoque": 8,​
   "valor": "73,30",​
   "tipo": "Garrafa 750ml"​
}

5. Deletando o documento de id 11.
>! DELETE /adega/_doc/11


## Desafio:
6. Faça o bulk insert com mias 3 bebidas com dados a sua escolha.
