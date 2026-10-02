---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/dashboards-overview/index.md
source_revision: c77015b329dc32a24556f9303c02d15e0f3cd4f4
translation_status: ready

description: Aprenda como os dashboards do Grafana são criados.
keywords:
  - grafana
  - dashboards
  - panel
  - data source
  - transform
  - query
labels:
  products:
    - cloud
    - enterprise
    - oss
menuTitle: Visão geral do dashboard
title: Visão geral dos dashboards do Grafana
weight: 390
refs:
  transform-data:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/panels-visualizations/query-transform-data/transform-data/
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/panels-visualizations/query-transform-data/transform-data/
---

# Visão geral dos dashboards do Grafana

Você já se perguntou o que é um dashboard?
No mundo da observabilidade, esse termo é frequentemente usado, mas o que ele
significa exatamente?
O conceito foi emprestado dos automóveis, onde um dashboard dá às pessoas
condutoras acesso aos controles necessários para operar um veículo.
Da mesma forma, os dashboards digitais nos ajudam a compreender e gerenciar
sistemas.
Este tópico explica como os dashboards do Grafana funcionam, permitindo que você
crie os seus próprios com maior facilidade.

A imagem a seguir ilustra um exemplo de dashboard do Grafana:

{{< figure src="/media/docs/grafana/dashboards-overview/complex-dashboard-example.png" max-width="750px" caption="Exemplo de dashboard do Grafana" >}}

Um dashboard do Grafana consiste em painéis que exibem dados em lindos gráficos,
tabelas e outras visualizações.
Esses painéis são criados usando componentes que transformam dados brutos de uma
fonte de dados em visualizações.
O processo envolve a passagem de dados por três etapas: um plugin, uma consulta
e uma transformação opcional.

A imagem abaixo exibe todos os portões, seguida de explicações detalhadas sobre
sua finalidade, uso e importância.

{{< figure src="/media/docs/grafana/dashboards-overview/dashboard-component-architecture.png" max-width="750px" caption="Arquitetura de componentes do dashboard" >}}

## Fontes de dados

Uma fonte de dados se refere a qualquer entidade que consiste em dados.
Pode ser um banco de dados SQL, o Grafana Loki, o Grafana Mimir ou uma API
baseada em JSON.
Pode até ser um simples arquivo CSV.
O primeiro passo para criar uma visualização de dashboard é selecionar a fonte
de dados que contém os dados necessários.

Pode ser difícil entender as diferenças entre as diversas fontes de dados, pois
cada uma possui sua própria estrutura e requer métodos de consulta distintos.
No entanto, em dashboards, você pode visualizar diferentes fontes de dados em
uma única visualização, facilitando a compreensão geral dos seus dados.

## Plugins

Um plugin do Grafana é um software que adiciona novas funcionalidades ao
Grafana.
Existem vários tipos, mas agora vamos abordar os _plugins de fonte de dados_.
A função de um plugin de fonte de dados do Grafana é receber uma consulta que
você deseja responder, recuperar os dados da fonte de dados e conciliar as
diferenças entre o modelo de dados da fonte de dados e o modelo de dados dos
dashboards do Grafana.
Isso é feito usando uma estrutura de dados unificada chamada
[data frame](https://grafana.com/developers/plugin-tools/key-concepts/data-frames).

Os dados que chegam ao plugin a partir da fonte de dados podem estar em vários
formatos diferentes (como JSON, linhas e colunas ou CSV), mas quando saem do
plugin e passam pelas etapas seguintes até a visualização, estão sempre em data
frames.

Atualmente, o Grafana oferece uma ampla gama de 155 fontes de dados que você
pode usar.
As opções mais comuns já estão pré-instaladas e acessíveis.
Antes de explorar outras opções, procure uma fonte de dados existente que atenda
às suas necessidades.
O Grafana atualiza a lista constantemente, mas se você não encontrar uma fonte
de dados adequada, pode navegar pelo
[catálogo de plugins](/grafana/plugins/?type=datasource) ou
[criar um plugin](/developers/plugin-tools).

## Consultas

As consultas permitem reduzir a totalidade dos seus dados a um conjunto de dados
específico, proporcionando uma visualização mais gerenciável.
Elas ajudam a responder perguntas sobre processos operacionais e de sistema.
Por exemplo, uma empresa com uma loja online pode querer determinar o número de
clientes que adicionam produtos aos seus carrinhos de compras.
Isso pode ser feito por meio de uma consulta que agrega métricas de acesso ao
serviço de carrinho de compras, revelando o número de pessoas usuárias que
acessam o serviço por segundo.

Ao trabalhar com fontes de dados, é crucial reconhecer que cada uma possui sua
própria linguagem de consulta distinta.
Por exemplo, as fontes de dados Prometheus usam
[PromQL](/blog/2020/02/04/introduction-to-promql-the-prometheus-query-language/),
enquanto [LogQL](/docs/loki/latest/logql/) é usada para logs, e alguns bancos de
dados específicos empregam SQL.
Uma consulta é a base de toda visualização no Grafana, e um dashboard pode usar
uma variedade de linguagens de consulta.

A imagem a seguir mostra o Query Editor associado à fonte de dados Prometheus.
A consulta `node_cpu_seconds_total` está escrita em PromQL e solicita apenas uma
métrica:

{{< figure src="/media/docs/grafana/dashboards-overview/example-query.png" max-width="750px" caption="Exemplo de consulta" >}}

## Transformações

Quando o formato dos dados em uma visualização não atende aos seus requisitos,
você pode aplicar uma [transformação](ref:transform-data) que manipula os dados
retornados por uma consulta.
Você pode não precisar transformar dados quando estiver começando, mas essas
transformações são poderosas e vale a pena mencioná-las.

Transformar dados é útil nos seguintes tipos de situações:

- Você deseja combinar dois campos, por exemplo, concatenando `Nome` e
  `Sobrenome` em um campo `Nome Completo`.
- Você tem dados CSV (todos em texto) e deseja converter o tipo de um campo
  (como extrair uma data ou um número de uma string).
- Você deseja filtrar, unir, mesclar ou executar outras operações semelhantes a
  SQL que podem não ser suportadas pela fonte de dados ou linguagem de consulta
  subjacente.

As transformações estão localizadas na guia **Transform** na caixa de diálogo de
edição de um painel.
Selecione e defina a transformação desejada.
A imagem a seguir mostra que você pode ter quantas transformações quiser, assim
como consultas.
Por exemplo, você pode encadear uma série de transformações que alteram um tipo
de dados, filtram resultados, organizam colunas e classificam o resultado em um
único pipeline de dados.
Sempre que o painel é atualizado, a transformação é aplicada aos dados mais
recentes da fonte de dados.

A imagem a seguir mostra a caixa de diálogo de transformação:

{{< figure src="/media/docs/grafana/dashboards-overview/example-transform-chain.png" max-width="750px" caption="Exemplo de cadeia de transformações" >}}

## Painéis

Após a obtenção, consulta e transformação dos dados, eles passam para um painel,
a etapa final no processo de criação de uma visualização no Grafana.
Um painel é um contêiner que exibe a visualização e fornece vários controles
para manipulá-la.
A configuração do painel permite especificar como você deseja visualizar os
dados.
Por exemplo, você usa um menu suspenso no canto superior direito do painel para
especificar o tipo de visualização desejada, como um gráfico de barras, um
gráfico de pizza ou um histograma.

As opções do painel permitem personalizar muitos aspectos da visualização, e as
opções variam conforme a visualização selecionada.
Os painéis também contêm consultas que especificam os dados que o painel está
visualizando.

A imagem a seguir mostra um painel de tabela sendo editado, as configurações do
painel exibindo a consulta na parte inferior e as opções do painel à direita.
Nesta imagem, você pode ver como a fonte de dados, o plugin, a consulta e o
painel se integram.

{{< figure src="/media/docs/grafana/dashboards-overview/example-table-panel.png" max-width="750px" caption="Exemplo de painel de tabela" >}}

A escolha da melhor visualização depende dos dados e de como você deseja
apresentá-los.
Para ver exemplos de dashboards em um só lugar, que você pode navegar e
inspecionar, consulte o [Grafana Play](https://play.grafana.org/), que apresenta
demonstrações de recursos e uma variedade de exemplos.

## Conclusão

Com a fonte de dados, o plugin, a consulta, a transformação e o modelo de painel
em mente, você agora pode visualizar facilmente qualquer dashboard do Grafana
que encontrar e imaginar como construir o seu próprio.

A criação de um dashboard do Grafana é um processo que começa com a determinação
dos requisitos do seu dashboard e a identificação de quais fontes de dados
atendem a esses requisitos.
Se você deseja integrar um banco de dados especializado a um dashboard do
Grafana, deve garantir que o plugin correto esteja instalado para que você possa
adicionar uma fonte de dados para usar com esse plugin.

Com a fonte de dados identificada e o plugin instalado, você pode escrever sua
consulta, transformar os dados e formatar a visualização de acordo com suas
necessidades.

Essa arquitetura de componentes é parte do que torna o Grafana tão poderoso e
versátil.
Graças ao plugin de fonte de dados e à abstração de data frame, qualquer fonte
de dados acessível pode ser utilizada com a mesma abordagem geral.
