# Laboratório 2 - Manipulação de dados no Elasticsearch:
**Criado por:** Anselmo Borges
**Ultima Atualização:** 09.12.2019

Nesse laboratório iremos realizar as seguintes instalações a manipulação dos dados do index Adega, vamos manipular as bebidas, estoque e preços. Trazer informações sobre determinadas bebidas e etc.

## Instalando o index adega
Vamos fazer o insert dos dados abaixo pelo Kibana para a criação do index Adega:<br>


```
PUT /adega/_doc/1
{
   "bebida": "Cerveja",
   "marca": "Skoll",
   "estoque": 300,
   "valor": "1,50",
   "tipo": "Lata 350ml"
}

PUT /adega/_doc/2 
{
   "bebida": "Cerveja",
   "marca": "Brahma",
   "estoque": 410,
   "valor": "1,39",
   "tipo": "lata 350ml"
}

PUT /adega/_doc/3
{
   "bebida": "Cerveja",
   "marca": "Stella Artois",
   "estoque": 516,
   "valor": "2,25",
   "tipo": "lata 269ml"
}

PUT /adega/_doc/4
{
   "bebida": "Cerveja",
   "marca": "Heineken",
   "estoque": 220,
   "valor": "3,35",
   "tipo": "lata 350ml"
}

PUT /adega/_doc/5
{
   "bebida": "Cerveja",
   "marca": "Heineken",
   "estoque": 256,
   "valor": "2,35",
   "tipo": "lata 250ml"
}

PUT /adega/_doc/6
{
   "bebida": "Vodka",
   "marca": "Smirnoff",
   "estoque": 12,
   "valor": "21,00",
   "tipo": "Garrafa 600ml"
}

PUT /adega/_doc/7
{
   "bebida": "Vodka",
   "marca": "Ciroc",
   "estoque": 25,
   "valor": "125,00",
   "tipo": "Garrafa 750ml"
}

PUT /adega/_doc/8
{
   "bebida": "Vodka",
   "marca": "Absolut",
   "estoque": 39,
   "valor": "69,90",
   "tipo": "Garrafa 750ml"
}

PUT /adega/_doc/9
{
   "bebida": "Vodka",
   "marca": "Ciroc",
   "estoque": 25,
   "valor": "125,00",
   "tipo": "Garrafa 750ml"
}

PUT /adega/_doc/10
{
   "bebida": "Wisky",
   "marca": "Ballantine's",
   "estoque": 66,
   "valor": "84,90",
   "tipo": "Garrafa 1 Litro"
}
``` 
## Exercicios:

1. Colha todas as informações sobre o index adega.
> GET /

2. Mostre as informações do documento de ID 1:
> GET /adega/_doc/1

3. Insira mais 2 registros de ID 11 e 12 com os dados abaixo:
* id: 11
   * bebida: Wisky
   * marca: Jack Daniel's
   * estoque: 12
   * valor: 109,90
   * tipo: Garrafa 750ml
* id: 12
   * bebida: Wisky
   * marca: Jack Daniel's
   * estoque: 8
   * valor: 69,90
   * tipo: Garrafa 750ml

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
