---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/glossary/index.md
source_revision: ada14df9fd1c9f8f7b93ce7ca33f5a60ecd4cc85
translation_status: ready

aliases:
  - ../basics/glossary/
  - ../getting-started/glossary/
  - ../guides/glossary/
description: Grafana glossary
keywords:
  - grafana
  - intro
  - glossary
  - dictionary
labels:
  products:
    - cloud
    - enterprise
    - oss
title: Glossário
weight: 850
---

# Glossário

Este tópico lista palavras e abreviações comumente usadas na documentação e na
comunidade do Grafana.

<table>
  <tr>
    <td style="vertical-align: top">app plugin (plugin de aplicação)</td>
    <td>
      Uma extensão do Grafana que permite às pessoas usuárias adicionar
      funcionalidades para aprimorar sua experiência, incluindo um conjunto de
      plugins de painel e de fonte de dados, além de páginas personalizadas.
      Veja também <i>data source plugin</i>, <i>panel plugin</i> e
      <i>plugin</i>.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">dashboard</td>
    <td>
      Um conjunto de um ou mais painéis, organizados e dispostos em uma ou mais
      linhas, que fornecem uma visão geral de informações relacionadas.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">data source (fonte de dados)</td>
    <td>
      Um arquivo, banco de dados ou serviço que fornece os dados.
      O Grafana suporta diversas fontes de dados por padrão e pode ser estendido
      para suportar fontes de dados adicionais por meio de plugins.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">data source plugin (plugin de fonte de dados)</td>
    <td>
      Estende o Grafana com suporte para fontes de dados adicionais.
      Veja também <i>data source</i>, <i>app plugin</i>, <i>panel plugin</i>,
      e <i>plugin</i>.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">exemplar</td>
    <td>
      Um exemplar é qualquer dado que sirva como um exemplo detalhado de uma das
      observações agregadas em uma métrica.
      Um exemplar contém o valor observado juntamente com um timestamp opcional
      e rótulos arbitrários, que normalmente são usados para referenciar um
      rastro.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">Explore</td>
    <td>
      O Explore permite que a pessoa usuária se concentre na criação de uma
      consulta.
      As pessoas usuárias podem refinar a consulta para retornar as métricas
      esperadas antes de criar um dashboard.
      Para obter mais informações, consulte o tópico
      <a href="https://grafana.com/docs/grafana/latest/explore">Explore</a>.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">exportar ou importar dashboard</td>
    <td>
      O Grafana inclui a capacidade de exportar seus dashboards para um arquivo
      contendo JSON.
      Pessoas membros da comunidade às vezes compartilham seus dashboards
      criados na
      <a href="https://grafana.com/grafana/dashboards">página Dashboards do Grafana</a>.
      Dashboards previamente exportados ou encontrados neste site podem ser
      importados por outras pessoas usuárias.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">exporter (exportador)</td>
    <td>
      Um exporter traduz os dados provenientes de uma fonte de dados para um
      formato que o Prometheus possa interpretar.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">Integration (Grafana Cloud)</td>
    <td>
      Cada Integration no Grafana Cloud usa o agente da nuvem para conectar sua
      fonte de dados ao Grafana Cloud para visualização.
      Observação: o Prometheus usa a palavra "integrations" para se referir a
      softwares que expõem métricas do Prometheus sem a necessidade de um
      exporter, que é um uso diferente da mesma palavra que usamos aqui.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">gráfico</td>
    <td>
      Uma visualização comumente usada que exibe dados como pontos, linhas ou
      barras.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top"><code>grafanactl</code></td>
    <td>
      Uma ferramenta de linha de comando que permite às pessoas usuárias
      autenticar, gerenciar múltiplos ambientes e executar tarefas
      administrativas por meio da API REST do Grafana.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">mixin</td>
    <td>
      Um mixin é um conjunto de dashboards do Grafana e regras e alertas do
      Prometheus, escritos em Jsonnet e agrupados em um pacote.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">painel</td>
    <td>
      Elemento básico do Grafana, composto por uma consulta e uma visualização.
      Pode ser movido e redimensionado em um dashboard.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">panel plugin (plugin de painel)</td>
    <td>
      Amplia o Grafana com opções de visualização adicionais.
      Veja também <i>panel</i>, <i>plugin</i>, <i>app plugin</i> e <i>data source plugin</i>.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">plugin</td>
    <td>
      Uma extensão do Grafana que permite às pessoas usuárias adicionar
      funcionalidades para melhorar a sua experiência.
      Veja também <i>app plugin</i>, <i>data source plugin</i> e
      <i>panel plugin</i>.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">query (consulta)</td>
    <td>
      Usada para solicitar dados de uma fonte de dados.
      A estrutura e o formato da consulta dependem da fonte de dados específica.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">time series (séries temporais)</td>
    <td>
      Uma série de medições, ordenadas por tempo.
      As séries temporais são armazenadas em fontes de dados e retornadas como
      resultado de uma consulta.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">trace (rastro)</td>
    <td>
      Um caminho de execução observado de uma requisição em um sistema
      distribuído.
      Para obter mais informações, consulte
      <a href="https://opentracing.io/docs/overview/what-is-tracing/">What is Distributed Tracing?</a>
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">transformação</td>
    <td>
      As transformações processam o conjunto de resultados de uma consulta antes
      de serem passadas para visualização.
      Para obter mais informações, consulte o tópico
      <a href="https://grafana.com/docs/grafana/latest/panels/transformations">Visão geral das transformações</a>.
    </td>
  </tr>
  <tr>
    <td style="vertical-align: top">visualização</td>
    <td>Uma representação gráfica dos resultados da consulta.</td>
  </tr>
</table>
