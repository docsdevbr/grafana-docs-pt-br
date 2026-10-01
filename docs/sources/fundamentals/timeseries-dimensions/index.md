---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/timeseries-dimensions/index.md
revision: 244ffad99d873bab23a1c749ed93384b6822f5b7
status: ready

aliases:
  - ../basics/timeseries-dimensions/
  - ../getting-started/timeseries-dimensions/
  - ../guides/timeseries-dimensions/
  - /docs/grafana-cloud/introduction/timeseries-dimensions/
description: Dimensões das séries temporais.
keywords:
  - grafana
  - intro
  - guide
  - concepts
  - timeseries
  - labels
labels:
  products:
    - cloud
    - enterprise
    - oss
title: Dimensões das séries temporais
weight: 500
refs:
  create-grafana-managed-rule:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/alerting/alerting-rules/create-grafana-managed-rule/#single-and-multi-dimensional-rule
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/alerting/alerting-rules/create-grafana-managed-rule/#single-and-multi-dimensional-rule
  time-series-databases:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/fundamentals/timeseries/#time-series-databases
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/fundamentals/timeseries/#time-series-databases
---

# Dimensões das séries temporais

Em [Introdução a séries temporais](ref:time-series-databases), o conceito de
_labels_ (rótulos), também chamados de _tags_, é apresentado:

> Outro recurso de um TSDB é a capacidade de filtrar medições usando _tags_.
> Cada ponto de dados é rotulado com uma tag que adiciona informações
> contextuais, como o local onde a medição foi feita.

Com dados de séries temporais, os dados geralmente contêm mais de uma série,
sendo um conjunto de múltiplas séries temporais.
Muitas fontes de dados do Grafana suportam esse tipo de dado.

{{< figure src="/static/img/docs/example_graph_multi_dim.png" class="docs-image--no-shadow" max-width="850px" alt="Temperatura por localização" >}}

O caso comum é a emissão de uma única consulta para uma medição com uma ou mais
propriedades adicionais como dimensões.
Por exemplo, consultar uma medição de temperatura juntamente com uma propriedade
de localização.
Nesse caso, várias séries são retornadas a partir de uma única consulta, e cada
série possui uma localização única como dimensão.

Para identificar séries únicas em um conjunto de séries temporais, o Grafana
armazena as dimensões em _labels_.

## Labels

Cada série temporal no Grafana pode ter labels.
Labels são um conjunto de pares chave/valor para identificar dimensões.
Exemplos de labels podem ser `{location=us}` ou
`{country=us,state=ma,city=boston}`.
Em um conjunto de séries temporais, a combinação do nome e das labels identifica
cada série.
Por exemplo, `temperature {country=us,state=ma,city=boston}` pode identificar a
série de valores de temperatura para a cidade de Boston, nos EUA.

Diferentes fontes de dados de séries temporais armazenam dimensões nativamente
ou possuem padrões de armazenamento comuns que permitem que os dados sejam
extraídos para dimensões.

Bancos de dados de séries temporais (TSDBs) geralmente oferecem suporte nativo à
dimensionalidade.
O Prometheus também armazena dimensões em _labels_.
Em TSDBs como Graphite ou OpenTSDB, o termo _tags_ é usado em vez de labels.

Em bancos de dados de tabelas, como SQL, essas dimensões são geralmente os
parâmetros `GROUP BY` de uma consulta.

## Múltiplas dimensões em formato de tabela

Em bancos de dados SQL ou similares que retornam respostas em formato de tabela,
as dimensões adicionais são geralmente representadas como colunas na tabela de
resposta da consulta.

### Dimensão única

Por exemplo, considere uma consulta como:

```sql
SELECT BUCKET(StartTime, 1h), AVG(Temperature) AS Temp, Location FROM T
  GROUP BY BUCKET(StartTime, 1h), Location
  ORDER BY time asc
```

Esta consulta retornaria uma tabela com três colunas, cada uma com um tipo de
dados: time, number e string, respectivamente:

| StartTime | Temp | Location |
| --------- | ---- | -------- |
| 09:00     | 24   | LGA      |
| 09:00     | 20   | BOS      |
| 10:00     | 26   | LGA      |
| 10:00     | 22   | BOS      |

O formato da tabela é uma série temporal _long_ (longa), também chamada de
_tall_ (alta).
Ela possui registros de data e hora repetidos e valores repetidos em Location.
Neste caso, temos duas séries temporais no conjunto que seriam identificadas
como `Temp {Local=LGA}` e `Temp {Local=BOS}`.

As séries temporais individuais do conjunto são extraídas usando a coluna do
tipo time `StartTime` como índice temporal da série, a coluna do tipo number
`Temp` como nome da série e o nome e os valores da coluna do tipo string
`Location` para construir as labels, como Location=LGA.

### Múltiplas dimensões

Se a consulta for atualizada para selecionar e agrupar por mais de uma coluna de
string, por exemplo, `GROUP BY BUCKET(StartTime, 1h), Location, Sensor`, uma
dimensão adicional será adicionada:

| StartTime | Temp | Location | Sensor |
| --------- | ---- | -------- | ------ |
| 09:00     | 24   | LGA      | A      |
| 09:00     | 24.1 | LGA      | B      |
| 09:00     | 20   | BOS      | A      |
| 09:00     | 20.2 | BOS      | B      |
| 10:00     | 26   | LGA      | A      |
| 10:00     | 26.1 | LGA      | B      |
| 10:00     | 22   | BOS      | A      |
| 10:00     | 22.2 | BOS      | B      |

Neste caso, as labels que representam as dimensões terão duas chaves baseadas
nas duas colunas do tipo string `Location` e `Sensor`.
Esses dados resultam em quatro séries: `Temp {Location=LGA,Sensor=A}`,
`Temp {Location=LGA,Sensor=B}`, `Temp {Location=BOS,Sensor=A}` e
`Temp {Location=BOS,Sensor=B}`.

{{< admonition type="note" >}}
Mais de uma dimensão é atualmente suportada apenas em consultas de Logs no
serviço Azure Monitor, a partir da versão 7.1.
{{< /admonition >}}

{{< admonition type="note" >}}
Múltiplas dimensões não são suportadas para mapear vários alertas no Grafana,
mas sim tratadas como múltiplas condições para um único alerta.
Para obter mais informações, consulte a documentação sobre
[criação de alertas com várias séries](ref:create-grafana-managed-rule).
{{< /admonition >}}

### Múltiplos valores

No caso de fontes de dados do tipo SQL, é possível selecionar mais de uma coluna
numérica, com ou sem colunas de string adicionais para serem usadas como
dimensões.
Por exemplo, `AVG(Temperature) AS AvgTemp, MAX(Temperature) AS MaxTemp`.
Isso, se combinado com várias dimensões, pode resultar em muitas séries.
A seleção de múltiplos valores destina-se atualmente apenas à visualização.

Informações técnicas adicionais sobre formatos de séries temporais tabulares e
como as dimensões são extraídas podem ser encontradas na
[documentação da pessoa desenvolvedora sobre data frames como séries temporais](https://grafana.com/developers/plugin-tools/key-concepts/data-frames#data-frames-as-time-series).
