---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/intro-to-prometheus/index.md
revision: c10dba5c63caca417bb819a2bbc665113e8bb2d2
status: ready

aliases:
  - ../basics/timeseries/
  - /docs/grafana-cloud/introduction/prometheus/
description: Introdução ao Prometheus
keywords:
  - grafana
  - intro
  - Prometheus
  - metrics
  - time series
labels:
  products:
    - cloud
    - enterprise
    - oss
title: O que é o Prometheus?
weight: 300
refs:
  prometheus:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/datasources/prometheus/
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/datasources/prometheus/
  build-dashboards:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/dashboards/build-dashboards/
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/dashboards/build-dashboards/
---

# O que é o Prometheus?

A observabilidade concentra-se na compreensão do estado interno dos seus
sistemas com base nos dados que eles produzem, o que ajuda a determinar se a sua
infraestrutura está saudável.
O Prometheus é uma tecnologia essencial para o monitoramento e observabilidade
de sistemas, mas o termo "Prometheus" pode ser confuso, pois é usado em
diferentes contextos.
Compreender os fundamentos do Prometheus, por que ele é valioso para a
observabilidade de sistemas e como as pessoas usuárias o utilizam na prática
ajudará você a entendê-lo melhor e a usar o Grafana.

O Prometheus surgiu em 2012 na SoundCloud porque as tecnologias existentes eram
insuficientes para atender às suas necessidades de observabilidade.
O Prometheus oferece um modelo de dados robusto e uma linguagem de consulta.
Além disso, o Prometheus é simples e escalável.
Em 2018, o Prometheus concluiu o programa de incubação da Cloud Native Computing
Foundation (CNCF) e hoje possui uma comunidade próspera.

## Prometheus como dados

O painel a seguir, em um dashboard do Grafana, mostra quanta largura de banda de
disco está sendo usada em um notebook Mac.
A linha verde representa as leituras (`reads`) do disco e a linha amarela
representa as gravações (`writes`).

{{< figure src="/media/docs/grafana/intro-prometheus/disk-io.png" max-width="750px" caption="Dashboard de E/S de disco" >}}

Dados como esses formam _séries temporais_.
O eixo X representa um instante no tempo e o eixo Y representa um número ou
medida; por exemplo, 5 megabytes por segundo.
Esse tipo de dado de série temporal aparece em todos os lugares no monitoramento
de sistemas, bem como em locais como gráficos de temperatura sazonal e preços de
ações.
Esses dados são simplesmente alguma medida (como o preço das ações de uma
empresa ou a E/S de disco) ao longo de uma série de instantes de tempo.

O Prometheus é uma tecnologia que coleta e armazena dados de séries temporais.
As séries temporais são fundamentais para o Prometheus; seu
[modelo de dados](https://prometheus.io/docs/concepts/data_model/) é organizado
em:

- _métricas_ que consistem em um _timestamp_ e uma _amostra_, a qual é o valor
  numérico, como quantos bytes de disco foram lidos ou o preço de uma ação.
- um conjunto de rótulos chamados _dimensões_, por exemplo, `job` e `device`.

Você pode armazenar dados de séries temporais em qualquer banco de dados
relacional; no entanto, esses sistemas não são desenvolvidos para armazenar e
consultar grandes volumes de dados de séries temporais.
O Prometheus e softwares similares fornecem ferramentas para compactar e
otimizar dados de séries temporais.

### Dashboard simples usando PromQL

A imagem do dashboard do Grafana a seguir mostra um gráfico de E/S de disco com
dados brutos do Prometheus, derivados de um notebook.

O campo **Metrics browser** contém a seguinte consulta:

`node_disk_written_bytes_total{job="integrations/macos-node", device!=""}`

Neste exemplo, o eixo Y mostra o número total de bytes gravados e o eixo X
mostra as datas e horários.
Conforme o notebook é usado, o número de bytes gravados aumenta ao longo do
tempo.
Abaixo do **Metrics browser**, há um counter (contador) que contabiliza o número
de bytes gravados ao longo do tempo.

{{< figure src="/media/docs/grafana/intro-prometheus/dashboard-example.png" max-width="750px" caption="Metrics browser e counter" >}}

A consulta é um exemplo simples de
[PromQL](/blog/2020/02/04/introduction-to-promql-the-prometheus-query-language/),
a Linguagem de Consulta do Prometheus.
A consulta identifica a métrica de interesse (`node_disk_written_bytes_total`) e
fornece dois rótulos (`job` e `device`).
O seletor de rótulo `job="integrations/macos-node"` filtra as métricas.
Ele reduz o escopo das métricas àquelas provenientes da tarefa de integração do
macOS e especifica que o rótulo “device” não pode estar vazio.
O resultado desta consulta é o fluxo bruto de números que o gráfico exibe.

Embora essa visualização forneça algumas informações sobre o desempenho do
sistema, ela não conta toda a história.
Uma visão mais clara do desempenho do sistema requer a compreensão da taxa de
variação, que demonstra a _velocidade com que os dados gravados estão sendo
alterados_.
Para monitorar adequadamente o desempenho do disco, você também precisa observar
picos de atividade que ilustrem se e quando o sistema está sob carga e se o
desempenho do disco está em risco.
O PromQL inclui uma função
[rate()](https://prometheus.io/docs/prometheus/latest/querying/functions/#rate)
que mostra a taxa média de aumento por segundo em intervalos de `5m` (5
minutos).
Essa visualização fornece uma visão muito mais clara do que está acontecendo com
o sistema.

{{< figure src="/media/docs/grafana/intro-prometheus/rate-function.png" max-width="750px" caption="Função rate do Prometheus" >}}

Um counter é apenas um tipo de métrica; é um número (como o total de bytes
gravados) que só aumenta.
O Prometheus
[suporta vários outros](https://prometheus.io/docs/concepts/metric_types/), como
o tipo de métrica `gauge` (medidor), que pode aumentar ou diminuir.

A visualização de gauge a seguir exibe o uso total de RAM em um computador.

{{< figure src="/media/docs/grafana/intro-prometheus/gauge-example.png" max-width="750px" caption="Visualização de um gauge" >}}

O terceiro tipo de métrica é chamado de `histogram` (histograma), que conta as
observações e as organiza em grupos configuráveis.
O exemplo a seguir exibe números de ponto flutuante agrupados em intervalos que
mostram a frequência com que cada um ocorreu.

{{< figure src="/media/docs/grafana/intro-prometheus/histogram-example.png" max-width="750px" caption="Visualização de um histogram" >}}

Esses conceitos fundamentais de séries temporais, métricas, rótulos e funções de
agregação são essenciais para o Grafana e para a observabilidade.

## Por que isso é valioso

Software e sistemas são um negócio complexo.
Às vezes, as coisas dão errado.
A observabilidade ajuda você a entender o estado de um sistema para que os
problemas possam ser identificados rapidamente e resolvidos proativamente.
E quando os problemas ocorrem, você pode receber alertas para diagnosticá-los e
solucioná-los dentro dos seus Objetivos de Nível de Serviço (SLOs).

Os
[três pilares da observabilidade](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/ch04.html)
são métricas, logs e rastros.
O Prometheus oferece suporte ao pilar de métricas.
Quando um software em um computador está lento, a observabilidade pode ajudar a
identificar se a CPU está saturada, se o sistema está sem memória ou se o disco
está gravando na velocidade máxima, para que você possa responder proativamente.

## Prometheus como software

O Prometheus não é apenas um formato de dados; também é considerado um
[kit de ferramentas de monitoramento e alerta de sistemas de código aberto](https://prometheus.io/docs/introduction/overview/).
Isso porque o Prometheus é um software, não apenas dados.

O Prometheus pode coletar e armazenar dados de métricas de softwares e
infraestruturas.
Coletar significa que o software Prometheus revisita periodicamente o mesmo
endpoint para verificar se há novos dados.
O Prometheus coleta dados de um software instrumentado com uma biblioteca
cliente.

Por exemplo, uma aplicação NodeJS pode configurar o
[prom-client](https://github.com/siimon/prom-client) para expor métricas
facilmente em um endpoint, e o Prometheus pode coletar dados desse endpoint
regularmente.
O Prometheus inclui diversas outras ferramentas em seu kit para instrumentar
suas aplicações.

## Prometheus como implantação

A primeira seção deste documento apresentou o conceito de Prometheus como dados
e como o modelo de dados e as métricas do Prometheus são organizados.
A segunda seção apresentou o conceito de Prometheus como software, usado para
coletar, processar e armazenar métricas.
Esta seção descreve como o Prometheus como dados e o Prometheus como software se
integram.

Considere o seguinte exemplo.
Suponha que uma aplicação 'MyApp' use um cliente Prometheus para expor métricas.
Uma abordagem para coletar dados de métricas é usar uma URL na aplicação que
aponte para um endpoint `http://localhost:3000/metrics` que produz dados de
métricas do Prometheus.

A imagem a seguir mostra as duas métricas associadas ao endpoint.
O texto HELP explica o significado da métrica e o texto TYPE indica o tipo de
métrica (neste caso, um gauge).
`MyAppnodejs_active_request_total` indica o número de requisições (neste caso,
`1`).
`MyAppnodejs_heap_size_total_bytes` indica o tamanho do heap reportado em bytes.
Há apenas dois números porque esses dados mostram o valor no momento em que
foram obtidos.

{{< figure src="/media/docs/grafana/intro-prometheus/endpoint-data.png" max-width="750px" caption="Exemplo de endpoint" >}}

As métricas de 'MyApp' estão disponíveis em um endpoint HTTP, mas como elas
chegam ao Grafana e, posteriormente, a um dashboard?
O processo de registrar e transmitir as leituras de uma aplicação ou parte da
infraestrutura é conhecido como _telemetria_.
A telemetria é fundamental para a observabilidade, pois ajuda a entender
exatamente o que está acontecendo na sua infraestrutura.
As métricas apresentadas anteriormente, por exemplo,
`MyAppnodejs_active_requests_total`, são dados de telemetria.

Para trazer as métricas para o Grafana, você pode usar o software Prometheus ou
o [Grafana Alloy](https://grafana.com/docs/alloy/latest/) para coletar as
métricas.
O Grafana Alloy coleta e encaminha os dados de telemetria para implantações de
código aberto da Grafana Stack, do Grafana Cloud ou do Grafana Enterprise, onde
seus dados podem ser analisados.
Por exemplo, você pode configurar o Grafana Alloy para coletar os dados de
'MyApp' a cada cinco segundos e enviar os resultados para o Grafana Cloud.

Os dados de métricas são apenas um tipo de dado de telemetria; os outros tipos
são logs e rastros.
Usar o Grafana Alloy pode ser uma ótima opção para enviar dados de telemetria,
pois, à medida que você expande suas práticas de observabilidade para incluir
logs e rastros, que o Grafana Alloy também suporta, você já tem uma solução
pronta.

A imagem a seguir ilustra como o Grafana Alloy funciona como intermediário entre
'MyApp' e o Grafana Cloud.

{{< figure src="/media/docs/alloy/flow-diagram-small-alloy.png" alt="Grafana Alloy" caption="Grafana Alloy" >}}

## Unindo tudo

A combinação do Prometheus com o Grafana Alloy permite controlar as métricas que
você deseja reportar, sua origem e destino.
Uma vez que os dados estejam no Grafana, eles podem ser armazenados em um banco
de dados Grafana Mimir.
Os dashboards do Grafana consistem em visualizações preenchidas com dados
consultados da fonte de dados Prometheus.
A consulta PromQL filtra e agrega os dados para fornecer as informações
necessárias.
Com essas etapas, passamos de números brutos, gerados por software, para o
Prometheus, entregues ao Grafana, consultados pelo PromQL e visualizados pelo
Grafana.

## Próximos passos?

Agora que você entende como as métricas do Prometheus funcionam, o que você vai
construir?

- Um ótimo próximo passo é [construir um dashboard](ref:build-dashboards) no
  Grafana e começar a transformar os dados brutos de telemetria do Prometheus em
  visões sobre o que está acontecendo com seus serviços e infraestrutura.
- Outro ótimo passo é aprender sobre o [Grafana Mimir](/oss/mimir/), que é
  essencialmente um banco de dados para dados do Prometheus.
  Se você está se perguntando como fazer isso funcionar para grandes volumes de
  métricas com muitos dados e consultas rápidas, confira o Grafana Mimir.
- Se você tiver interesse em trabalhar diretamente com dados do Prometheus no
  Grafana, consulte a documentação da
  [fonte de dados Prometheus](ref:prometheus) ou confira as
  [noções básicas de PromQL](https://prometheus.io/docs/prometheus/latest/querying/basics/).
