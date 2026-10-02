---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/exemplars/index.md
source_revision: a5ee2124401d82051e7f2351d2b89e5f486659e6
translation_status: ready

aliases:
  - ../basics/exemplars/
  - ../basics/exemplars/view-exemplars/
description: Introdução aos exemplares.
keywords:
  - grafana
  - concepts
  - exemplars
  - prometheus
labels:
  products:
    - cloud
    - enterprise
    - oss
menuTitle: Exemplars
title: Introdução aos exemplares
weight: 800
---

# Introdução aos exemplares

Um exemplar é um rastro específico que representa uma medição feita em um
determinado intervalo de tempo.
Enquanto as métricas são excelentes para fornecer uma visão agregada do seu
sistema, os rastros oferecem uma visão detalhada de uma única requisição; os
exemplares são uma maneira de conectar os dois.

Suponha que o site da sua empresa esteja experimentando um aumento repentino no
volume de tráfego.
Embora mais de 80% das pessoas usuárias consigam acessar o site em menos de dois
segundos, algumas pessoas usuárias estão experimentando um tempo de resposta
maior do que o normal, resultando em uma experiência ruim.

Para identificar os fatores que contribuem para a latência, você deve comparar
um rastro de uma resposta rápida com um rastro de uma resposta lenta.
Dada a vasta quantidade de dados em um ambiente de produção típico, essa é uma
tarefa extremamente trabalhosa e demorada.

Use exemplares para isolar problemas na sua distribuição de dados, identificando
rastros de consultas que apresentam alta latência em um determinado intervalo de
tempo.
Após localizar o problema de latência em alguns exemplares de rastro, você pode
combiná-los com informações adicionais do sistema ou propriedades de localização
para realizar uma análise de causa raiz mais rapidamente, levando a soluções
rápidas para problemas de desempenho.

O suporte para exemplares está disponível apenas para a fonte de dados do
Prometheus.
Após habilitar a funcionalidade, os dados de exemplares ficam disponíveis por
padrão.
Para obter mais informações sobre a configuração de exemplares e como
habilitá-los, consulte a seção Exemplares em
[Opções de configuração do Prometheus](https://grafana.com/docs/grafana/latest/datasources/prometheus/configure/#configuration-options).

O Grafana exibe exemplares com uma métrica na visualização Explore e nos
dashboards.
Cada exemplar é exibido como uma estrela destacada.
Você pode passar o cursor sobre um exemplar para visualizar o ID único do
rastro, que é uma combinação de um par chave-valor.
Para investigar mais a fundo, clique no botão azul ao lado da propriedade
`traceID`.

{{< figure src="/media/docs/grafana/exemplars/screenshot-exemplars.png" class="docs-image--no-shadow" max-width= "750px" caption="Captura de tela mostrando a janela de detalhes de um exemplar" >}}

Consulte [Visualizar dados de exemplares](#view-exemplar-data) para obter
instruções sobre como detalhar e visualizar os detalhes do rastro de exemplares
a partir de métricas e logs.
Para saber mais sobre exemplares, consulte a postagem do blog
[Introdução aos exemplares, que habilitam o rastreamento distribuído do Grafana Tempo em grande escala](/blog/2021/03/31/intro-to-exemplars-which-enable-grafana-tempos-distributed-tracing-at-massive-scale/).

## Visualizar dados de exemplares

Quando o suporte a exemplares está habilitado para uma fonte de dados do
Prometheus, você pode visualizar os dados de exemplares na visualização Explore
ou nos detalhes de log do Loki.

### No Explore

O Explore visualiza os rastros de exemplares como estrelas destacadas junto aos
dados de métricas.
Para obter mais informações sobre como o Explore visualiza os dados de rastros,
consulte [Rastreamento no Explore](../../explore/trace-integration/).

Para examinar os detalhes de um rastro de exemplar:

1. Posicione o cursor sobre um exemplar (estrela destacada).
   Dependendo da fonte de dados do rastro que você estiver usando, poderá ver um
   botão azul com o rótulo `Query with <NOME DA FONTE DE DADOS>`.
   No exemplo a seguir, a fonte de dados de rastreamento é o Tempo.

   {{< figure src="/media/docs/grafana/exemplars/screenshot-exemplar-details.png" class="docs-image--no-shadow" max-width= "350px" caption="Captura de tela mostrando detalhes do exemplar" >}}

1. Clique na opção **Query with Tempo** ao lado da propriedade `traceID`.
   Os detalhes do rastro, incluindo os spans (trechos) dentro do rastro, são
   listados em um painel separado à direita.

   {{< figure src="/media/docs/grafana/exemplars/screenshot-exemplar-explore-view.png" class="docs-image--no-shadow" max-width= "900px" caption="Visualização do Explorer com painel mostrando detalhes do rastro" >}}

Para obter mais informações sobre como detalhar e analisar os detalhes do rastro
e dos spans, consulte a seção
[Analisar detalhes do rastro e dos spans](#analisar-detalhes-do-rastro-e-dos-spans).

### Nos logs

Você também pode visualizar detalhes do rastro de exemplares dos logs do Loki no
Explore.
Use expressões regulares nos links de campos derivados do Loki para extrair as
informações de `traceID`.
Agora, ao expandir os logs do Loki, você verá a propriedade `traceID` na seção
**Detected fields**.
Para saber mais sobre como extrair parte de uma mensagem de log para um link
interno ou externo, consulte
[usando campos derivados no Loki](../../explore/logs-integration/).

Para visualizar os detalhes de um rastro de exemplar:

1. Expanda uma linha de log e role para baixo até a seção `Fields`.
   Dependendo da fonte de dados do seu backend de rastros, você poderá ver um
   botão azul com o rótulo `<NOME DA FONTE DE DADOS>`.

1. Clique no botão azul ao lado da propriedade `traceID`.
   Normalmente, ele contém o nome da fonte de dados do backend.
   No exemplo a seguir, a fonte de dados de rastreamento é Tempo.
   Os detalhes do rastro, incluindo os spans dentro do rastro, são listados em
   um painel separado à direita.

{{< figure src="/media/docs/grafana/exemplars/screenshot-exemplar-loki-logs.png" class="docs-image--no-shadow" max-width= "750px" caption="Visualização Explorer com painel mostrando detalhes do rastro" >}}

Para obter mais informações sobre como detalhar e analisar os detalhes do rastro
e dos spans, consulte a seção
[Analisar detalhes do rastro e dos spans](#analisar-detalhes-do-rastro-e-dos-spans).

### Analisar detalhes do rastro e dos spans

Este painel mostra os detalhes do rastro em diferentes segmentos.

- O segmento superior exibe o ID do rastro para indicar que os resultados da
  consulta correspondem ao rastro específico.

  Você pode adicionar mais rastros aos resultados usando o botão `Add query`.

- O próximo segmento mostra todo o span do rastro específico como uma faixa
  estreita.
  Todos os níveis do rastro, desde o cliente até a consulta ao banco de dados,
  são exibidos, o que proporciona uma visão geral da distribuição de tempo em
  todas as camadas pelas quais a requisição HTTP foi processada.

  1. Você pode clicar dentro desta faixa de visualização para exibir uma
     visualização ampliada de um segmento de tempo menor dentro do span.
     Essa visualização ampliada aparece no segmento inferior do painel.

  2. Na visualização ampliada, você pode expandir ou recolher os vários níveis
     do rastro para detalhar o span específico de interesse.

     Por exemplo, se a faixa de visualização mostrar que a maioria da latência
     estava na camada da aplicação, você pode expandir o rastro para baixo na
     camada da aplicação para investigar o problema mais a fundo.
     Para expandir uma camada específica do span, clique no ícone à esquerda.
     O mesmo botão pode recolher um span expandido.

- Para ver os detalhes do span em qualquer nível, clique no próprio span.

  Isso exibe metadados adicionais associados ao span.
  Os metadados em si são inicialmente exibidos em uma faixa estreita, mas você
  pode ver mais detalhes clicando na faixa de metadados.

  {{< figure src="/media/docs/grafana/exemplars/screenshot-exemplar-span-details.png" class="docs-image--no-shadow" max-width= "600px" caption="Detalhes do span" >}}
