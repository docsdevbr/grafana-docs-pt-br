---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/getting-started/first-dashboards/get-started-grafana-influxdb.md
revision: 83e190a160cc4ea88e606b9d43b09d4ab8ceb1fa
status: ready

aliases:
  - ../../../getting-started/getting-started-influxdb/ # /docs/grafana/latest/getting-started/getting-started-influxdb
  - ../../../getting-started/get-started-grafana-influxdb/ # /docs/grafana/latest/getting-started/get-started-grafana-influxdb
description: Aprenda a criar seu primeiro dashboard do InfluxDB no Grafana.
labels:
  products:
    - enterprise
    - oss
title: Comece a usar o Grafana e o InfluxDB
weight: 400
---

# Comece a usar o Grafana e o InfluxDB

{{< docs/shared lookup="influxdb/intro.md" source="grafana" version="<GRAFANA VERSION>" >}}

#### Baixe o InfluxDB

Você pode [baixar o InfluxDB](https://portal.influxdata.com/downloads/) e
instalá-lo localmente ou pode se inscrever no
[InfluxDB Cloud](https://www.influxdata.com/products/influxdb-cloud/).
Instaladores para Windows não estão disponíveis para algumas versões do InfluxDB.

#### Instale outros softwares do InfluxDB

[Instale o Telegraf](https://docs.influxdata.com/telegraf/v1.18/introduction/installation/).
Esta ferramenta é um agente que ajuda você a importar métricas para o InfluxDB.
Para obter mais informações, consulte a
[documentação do Telegraf](https://docs.influxdata.com/telegraf/v1.18/).

Se você optar por usar o InfluxDB Cloud, deverá
[baixar e instalar a CLI do InfluxDB Cloud](https://portal.influxdata.com/downloads/).
Essa ferramenta permite enviar instruções de linha de comando para sua conta na
nuvem.
Para obter mais informações, consulte a
[documentação da CLI do InfluxDB](https://docs.influxdata.com/influxdb/cloud/write-data/developer-tools/influx-cli/).

#### Importe dados para o InfluxDB

Se você baixou e instalou o InfluxDB em sua máquina local, use o recurso
[Início Rápido](https://docs.influxdata.com/influxdb/v2.0/write-data/#quick-start-for-influxdb-oss)
para visualizar as métricas do InfluxDB.

Se você estiver usando uma conta na nuvem, os assistentes o guiarão pelo
processo inicial.
Para obter mais informações, consulte
[Configurar o Telegraf](https://docs.influxdata.com/influxdb/cloud/write-data/no-code/use-telegraf/#configure-telegraf).

##### Observação para pessoas usuárias do Windows:

Usuários do Windows podem precisar fazer ajustes adicionais.
Consulte as instruções específicas na documentação do InfluxData e na postagem
do blog
[Usando o Telegraf no Windows](https://www.influxdata.com/blog/using-telegraf-on-windows/).
O modelo padrão de monitoramento de sistema no InfluxDB Cloud não é compatível
com o Windows.
Pessoas usuárias do Windows que utilizam o InfluxDB Cloud para monitorar seus
sistemas precisarão usar o
[Template de Monitoramento de Sistema para Windows](https://github.com/influxdata/community-templates/tree/master/windows_system).

#### Adicione sua fonte de dados do InfluxDB ao Grafana

Você pode ter mais de uma fonte de dados do InfluxDB definida no Grafana.

1. Siga as instruções gerais para
   [adicionar uma fonte de dados](../../datasources/#add-a-data-source).
1. Decida se você usará InfluxQL ou Flux como linguagem de consulta.
   - [Configure a fonte de dados](../../datasources/influxdb/#configure-the-data-source)
     para a linguagem de consulta escolhida.
     Cada linguagem de consulta possui suas próprias configurações de fonte de
     dados exclusivas.
   - Para consultar recursos específicos de cada idioma, consulte a
     [documentação do editor de consultas](../../datasources/influxdb/query-editor/)
     da fonte de dados.

##### Guias do InfluxDB

O InfluxDB publica guias para conectar diferentes versões do seu produto ao
Grafana.

- **InfluxDB OSS ou Enterprise 1.8+.**
  Para ativar o Flux, consulte
  [Configurar o InfluxDB](https://docs.influxdata.com/influxdb/v1.8/administration/config/#flux-enabled-false).
  Selecione sua versão do InfluxDB no canto superior direito.
- **InfluxDB OSS ou Enterprise 2.x.**
  Consulte [Usar o Grafana com o InfluxDB](https://docs.influxdata.com/influxdb/v2.0/tools/grafana/).
  Selecione sua versão do InfluxDB no canto superior direito.
- **InfluxDB Cloud.**
  Consulte
  [Usar o Grafana com o InfluxDB Cloud](https://docs.influxdata.com/influxdb/cloud/tools/grafana/).

##### Dicas importantes

- Certifique-se de que seu token do Grafana tenha permissão de leitura.
  Caso contrário, você receberá um erro de autenticação e não conseguirá
  conectar o Grafana ao InfluxDB.
- Evite apóstrofos e outros caracteres não padronizados nos nomes de buckets e
  tokens.
- Se o nome da organização ou do bucket não funcionar, tente usar o número de
  ID.
- Se você alterar o nome do seu bucket no InfluxDB, também deverá alterá-lo no
  Grafana e no seu arquivo .conf do Telegraf.

#### Adicione uma consulta

Esta etapa varia dependendo da linguagem de consulta que você selecionou ao
configurar sua fonte de dados no Grafana.

##### Linguagem de consulta InfluxQL

No editor de consultas, clique em **select measurement**.

![Consulta InfluxQL](/static/img/docs/influxdb/influxql-query-7-5.png)

O Grafana exibe uma lista de séries possíveis.
Clique em uma para selecioná-la e o Grafana criará um gráfico com os dados
disponíveis.
Se não houver dados para exibir, tente outra seleção ou verifique sua fonte de
dados.

##### Linguagem de consulta Flux

Crie uma consulta Flux simples.

1. [Adicione um painel](../../dashboards/build-dashboards/create-dashboard/).
1. No editor de consultas, selecione sua fonte de dados InfluxDB-Flux.
   Para obter mais informações, consulte
   [Consultas](../../panels-visualizations/query-transform-data/).
1. Selecione a visualização **Table**.
1. No campo de texto do editor de consultas, digite `buckets()` e clique fora do
   editor de consultas.

Esta consulta genérica retorna uma lista de buckets.

![Consulta Flux](/static/img/docs/influxdb/flux-query-7-5.png)

Você também pode criar consultas Flux na visualização Explore do InfluxDB.

1. No seu navegador, faça login na interface nativa do InfluxDB (a versão OSS
   geralmente é algo como http://localhost:8086 ou, para uso com o InfluxDB
   Cloud: https://cloud2.influxdata.com).
1. Clique em **Explore** para abrir o Data Explorer.
1. O Data Explorer do InfluxDB oferece dois mecanismos para criar consultas
   Flux: um editor gráfico de consultas e um editor de scripts.
   Usando o editor gráfico de consultas,
   [crie uma consulta](https://docs.influxdata.com/influxdb/cloud/query-data/execute-queries/data-explorer/).
   Ela terá uma aparência semelhante a esta:

   ![Consulta do InfluxDB Explore](/static/img/docs/influxdb/influx-explore-query-7-5.png)

1. Clique em **Script Editor** para visualizar o texto da consulta e copie todas
   as linhas do seu código Flux, que ficará semelhante a este:

   ![Script Editor do InfluxDB Explore](/static/img/docs/influxdb/explore-query-text-7-5.png)

1. No Grafana,
   [adicione um painel](../../dashboards/build-dashboards/create-dashboard/) e
   cole seu código Flux no editor de consultas.
1. Clique em **Apply**.
   Seu novo painel deverá estar visível com os dados da sua consulta Flux.

#### Verifique as métricas do InfluxDB no Grafana Explore

Na sua instância do Grafana, acesse a visualização [Explore](../../explore/) e
crie consultas para experimentar com as métricas que deseja monitorar.
Aqui você também pode depurar problemas relacionados à coleta de métricas.

#### Comece a criar dashboards

Pronto!
Use o Explore e o Data Explorer para experimentar com seus dados e adicione as
consultas que você gostar ao seu dashboard como painéis.
Divirta-se!

Aqui estão alguns recursos para aprender mais:

- Documentação do Grafana:
  [Fonte de dados InfluxDB](../../datasources/influxdb/)
- Documentação do InfluxDB:
  [Comparação de Flux vs InfluxQL](https://docs.influxdata.com/influxdb/v1.8/flux/flux-vs-influxql/)
