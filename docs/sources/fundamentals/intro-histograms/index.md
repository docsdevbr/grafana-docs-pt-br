---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/intro-histograms/index.md
revision: 62fc451f7ae7cb8c2829442f0e6c57fae8bccdbd
status: ready

aliases:
  - ../basics/intro-histograms/
  - ../getting-started/intro-histograms/
description: Uma introdução a histogramas e mapas de calor.
keywords:
  - grafana
  - heatmap
  - panel
  - documentation
  - histogram
labels:
  products:
    - cloud
    - enterprise
    - oss
menuTitle: Histogramas e mapas de calor
title: Introdução a histogramas e mapas de calor
weight: 650
refs:
  heatmap:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/panels-visualizations/visualizations/heatmap/
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/panels-visualizations/visualizations/heatmap/
  histogram:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/panels-visualizations/visualizations/histogram/
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/panels-visualizations/visualizations/histogram/
---

# Introdução a histogramas e mapas de calor

Um histograma é uma representação gráfica da distribuição de dados numéricos.
Ele agrupa valores em intervalos (às vezes também chamados de classes) e, em
seguida, conta quantos valores se enquadram em cada intervalo.

Em vez de representar graficamente os valores reais, os histogramas representam
graficamente os intervalos.
Cada barra representa um intervalo, e a altura da barra representa a frequência
(como a contagem) dos valores que se enquadram naquele intervalo.

## Exemplo de histograma

Este _histograma_ mostra a distribuição de valores de algumas séries temporais.
Você pode notar facilmente que a maioria dos valores se concentra entre 240 e
300, com um pico entre 260 e 280.

![Exemplo de histograma](/static/img/docs/v43/heatmap_histogram.png)

Aqui está um exemplo mostrando a distribuição de altura das pessoas.

{{< figure src="/static/img/docs/histogram-panel/histogram-example-v8-0.png" max-width="625px" caption="Exemplo de gráfico de barras" >}}

Para obter mais informações sobre as opções de visualização de histogramas,
consulte [Histograma](ref:histogram).

Os histogramas analisam apenas a _distribuição de valores_ em um intervalo de
tempo específico.
O problema com os histogramas é que não é possível observar tendências ou
mudanças na distribuição ao longo do tempo.
É aí que os mapas de calor se tornam úteis.

## Mapas de calor

Um _mapa de calor_ é semelhante a um histograma, mas ao longo do tempo, onde
cada intervalo de tempo representa seu próprio histograma.
Em vez de usar a altura da barra como representação da frequência, ele usa
células e colore a célula proporcionalmente ao número de valores no intervalo.

Neste exemplo, você pode notar claramente quais valores são mais comuns e como
eles se comportam ao longo do tempo.

![Exemplo de mapa de calor](/static/img/docs/v43/heatmap_histogram_over_time.png)

Para obter mais informações sobre as opções de visualização de mapas de calor,
consulte [Mapa de calor](ref:heatmap).

## Dados pré-agrupados

Existem diversas fontes de dados que suportam histogramas ao longo do tempo,
como o Elasticsearch (usando uma agregação de intervalos de histograma) ou o
Prometheus (com o tipo de métrica
[histogram](https://prometheus.io/docs/concepts/metric_types/#histogram) e a
opção _Format as_ definida como Heatmap).
Mas, em geral, qualquer fonte de dados pode ser usada, desde que atenda ao
requisito de retornar séries com nomes que representem os limites dos intervalos
ou de retornar séries classificadas pelos limites em ordem crescente.

## Dados brutos vs. agregados

Se você usar o mapa de calor com dados de séries temporais regulares (não
pré-agrupados), é importante ter em mente que seus dados geralmente já estão
agregados pelo seu backend de séries temporais.
A maioria das consultas de séries temporais não retorna dados de amostra brutos,
mas inclui um agrupamento por intervalo de tempo ou limite de maxDataPoints,
juntamente com uma função de agregação (geralmente a média).

Tudo isso depende do intervalo de tempo da sua consulta, é claro.
Mas o ponto importante é saber que o agrupamento de histograma que o Grafana
realiza pode ser feito em dados já agregados e calculados em média.
Para obter mapas de calor mais precisos, é melhor fazer o agrupamento durante a
coleta de métricas ou armazenar os dados no Elasticsearch ou em qualquer outra
fonte de dados que suporte o agrupamento de histograma em dados brutos.

Se você remover ou diminuir o agrupamento por tempo (ou aumentar o
maxDataPoints) em sua consulta para retornar mais pontos de dados, seu mapa de
calor será mais preciso, mas isso também pode sobrecarregar muito a CPU e a
memória do seu navegador, possivelmente causando travamentos ou falhas se o
número de pontos de dados se tornar excessivamente grande.
