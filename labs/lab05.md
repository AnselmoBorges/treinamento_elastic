# Laboratório 3 - Realizando Consultas
**Criado por:** Anselmo Borges <br>
**Ultima Atualização:** 08.01.2020

Nesse laboratório iremos realizar algumas agregações conforme os capítulos 8, 9 e 10.

## Agregações de métricas:
1. Usando o index de logs do servidores importado pelo filebeat consulte qual foi a consulta mais rápida:

```
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
```
2. Agora veja qual foi a mais demorada:
```
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
```

3. Quando você quiser calcular metricas como (min, max, avg e sum) você pode usar o stats como agregação, veja essa informação "stat" no runtime_ms:
```
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
```
Todos os resultados são trazidos na mesma consulta.

4. Vamos usar o "percentile". Calcule o tempo médio de execução e verifique se 95% das solicitações demoram menos de 500 milissegundos.
```
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
```

5. Quantas solicitações de URL distintas foram registradas nos índices logs_server *? As solicitações de URL são indexadas no campo originalUrl. Você deve obter cerca de 37.000 como resultado.

```
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
```
