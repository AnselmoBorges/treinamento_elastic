# Laboratório 4 - Refinando consultas
**Criado por:** Anselmo Borges<br>
**Ultima Atualização:** 09.02.2020

Nesse laboratório iremos realizar algumas consultas no schema blogs criado no lab anterior:

## Observando o _score dos documentos trazidos:
Como dito no capitulo, realizamos as consultas e cada documento tem um peso de score de acordo com o algoritmo BM29 que ranqueia a relevancia de cada um. Vamos fazer uma consulta básica e observer onte estão meu:
* Max Score
* Score de cada documento.

    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->
            
        GET blogs/_search
         {
          "query": {
            "match": {
              "content": "elastic"
            }
          }
        }

    </details>

## Mudando a forma de consulta do match de or para and:
Conforme o capitulo informa, quando fazemos uma consulta com  match e usamos mais de uma palavra, por exemplo, "quero cerveja" ele usa por padrão o "or" que busca ou "quero" ou "cerveja", para mudar isso precisamos mudar o operador.

Faça uma busca onde no titulo teremos "elastic stack" e no resultado venham documentos com "elastic" e "stack"
   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET blogs/_search
        {
          "query": {
            "match": {
              "title": {
                "query": "elastic stack",
                "operator": "and"
              }
            }
          }
        }
  </details>

## Usando o Slop com frases:
Vamos usar essa mesma busca, utilizando slop ao invés do operador para uma palavra apenas. Busque pela frase "elastic stack" no titulo.

   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET blogs/_search
        {
          "query": {
            "match_phrase": {
              "title": {
                "query": "elastic stack",
                "slop": 1
              }
            }
          }
        }
  </details>

## Realize uma busca de uma string em vários campos:
Vamos realizar uma busca da palavra "open" em mais de uma campo, no nosso caso, os campos title e content:

   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET blogs/_search
        {
          "query": {
            "multi_match": {
              "query": "open",
              "fields": [
                "title",
                "content"
              ]
            }
          }
        }
  </details>

## Buscando em vários campos uma frase:
Vamos alterar a query anterior buscando pela frase "open source".

   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET blogs/_search
        {
          "query": {
            "multi_match": {
              "query": "open source",
              "fields": [
                "title",
                "content"
              ],
              "type" : "phrase"
            }
          }
        }
  </details>

## Potencializando a busca em um campo especifico.
Suponhamos que nessa mesma consulta eu precise que os documentos que forem encontrados em "title" venham primeiro na ordem de score:

   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET blogs/_search
        {
          "query": {
            "multi_match": {
              "query": "open source",
              "fields": [
                "title^2",
                "content"
              ],
              "type" : "phrase"
            }
          }
        }
  </details>

## Usando a substituição:
Suponhamos que eu tenha digitado a busca errada em alguns caracteres, por exemplo, "oven saurce" e estou querendo buscar por "open source" porem, eu tenho um metodo de não setar numeros e o fuzziness se virar automaticamente. Qual seria?

   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET blogs/_search
        {
          "_source": "title",
          "query": {
            "match": {
              "title": {
                "query" : "oven sauce",
                "fuzziness": "auto"
              }
            }
          }
        }
  </details>

## Usando o bool para busca de mais de um valor em mais de um campo:

Vamos começar a usar o bool agora, nesse caso eu quero que ele busque por "improvements", "performance" e "optimizations" em content.

   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET blogs/_search 
        {
          "query": {
            "bool": {
              "must": [
                {
                  "match": {
                    "content": "performance"
                  }
                },
                {
                  "match": {
                    "content": "optimizations"
                  }
                },
                {
                  "match": {
                    "content": "improvements"
                  }
                }
                ]
            }
          }
        }
  </details>

## Alterando o Ranking na busca booleana:
Quero que ele melhore o ranking dos que forem encontrados com "improvements" em "content".

   <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->
        GET blogs/_search 
        {
          "query" : {
            "bool": {
              "must": [
                {
                  "match": {
                    "content": "performance"
                  }
                },
                {
                  "match": {
                    "content": "optimizations"
                  }
                }
              ],
              "should": [
                {
                  "match" : {
                    "content": "improvements"
                  }
                }
              ]
            }
          }
        }
  </details>

## Usando o must_not
Quero que na mesma consulta eu não traga os itens com "optimizations" no "title"
> 
GET blogs/_search 
{
  "query" : {
    "bool": {
      "must": [
        {
          "match": {
            "content": "performance"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "title": "optimizations"
          }
        }
      ], 
      "should": [
        {
          "match" : {
            "content": "improvements"
          }
        }
      ]
    }
  }
}
