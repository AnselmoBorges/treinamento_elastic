# Laboratório 3 - Realizando Consultas
**Criado por:** Anselmo Borges <br>
**Ultima Atualização:** 08.01.2020

Nesse laboratório iremos realizar algumas agregações conforme os capítulos 8, 9 e 10.

## Agregações de métricas:
1. Usando o index de logs do servidores importado pelo filebeat consulte qual foi a consulta mais rápida:

    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET logs_server*/_search
        {
          "size": 0,
          "aggs": {
            "fastest_request_time": {
              "min": {
                "field": "runtime_ms"
              }
            }
          }
        }
    </details>

2. Agora veja qual foi a mais demorada:

    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET logs_server*/_search
        {
          "size": 0,
          "aggs": {
            "slowest_request_time": {
              "max": {
                "field": "runtime_ms"
              }
            }
          }
        }
    </details>

3. Quando você quiser calcular metricas como (min, max, avg e sum) você pode usar o stats como agregação, veja essa informação "stat" no runtime_ms:
    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET logs_server*/_search
        {
          "size": 0,
          "aggs": {
            "request_time_stats": {
              "stats": {
                "field": "runtime_ms"
              }
            }
          }
        }
    </details>

**Todos os resultados são trazidos na mesma consulta.**

4. Vamos usar o "percentile". Calcule o tempo médio de execução e verifique se 95% das solicitações demoram menos de 500 milissegundos.
    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET logs_server*/_search
        {
          "size": 0,
          "aggs": {
            "runtime_median_and_95": {
              "percentiles": {
                "field": "runtime_ms",
                "percents": [
                  50,
                  95
                ]
              }
            }
          }
        }
    </details>

5. Quantas solicitações de URL distintas foram registradas nos índices logs_server *? As solicitações de URL são indexadas no campo originalUrl. Você deve obter cerca de 37.000 como resultado.

    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->

        GET logs_server*/_search
        {
          "size": 0,
          "aggs": {
            "my_url_value_count": {
              "cardinality": {
                "field": "originalUrl.keyword"
              }
            }
          }
        }
    </details>

## Agregações com Buckets
Agregações quando você deseja separar por periodo, range e outras.

6. Quantas solicitações existem para cada um 
dos 6 valores status_code?
    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->
            
        GET logs_server*/_search
             {
               "size": 0,
               "aggs": {
                 "status_code_buckets": {
                   "terms": {
                     "field" : "status_code"
                   }
                 }
               }
             }
    </details>

7. Uma agregação de termos é classificada por doc_count por padrão. Modifique sua pesquisa anterior para que seus termos sejam classificados em ordem alfabética.
    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->
            
        GET logs_server*/_search
        {
          "size": 0,
          "aggs": {
            "status_code_buckets": {
              "terms": {
                "field": "status_code",
                "order": {
                  "_key": "asc"
                }
              }
            }
          }
        }
    </details>

8. Quantos logs foram gerados por semana?
    <details>
      <summary>Resposta</summary>
        <!-- language: lang-json -->
            
        GET logs_server*/_search
        {
          "size": 0,
          "aggs": {
            "logs_by_week": {
              "date_histogram": {
                "field": "@timestamp",
                "interval": "week"
              }
            }
          }
        }
    </details>

  ## Fim de laboratório