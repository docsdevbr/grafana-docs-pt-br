---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/introduction/_index.md
revision: e48eaa567edcc5ea9cda19c2042dcfc83d02056a
status: ready

aliases:
  - guides/what-is-grafana/
  - oss-details/
description: Saiba mais sobre Grafana OSS, Grafana Enterprise e Grafana Cloud.
labels:
  products:
    - cloud
    - enterprise
    - oss
title: Sobre o Grafana
weight: 5
---

# Sobre o Grafana

O [software de código aberto Grafana](/oss/) permite consultar, visualizar,
configurar alertas e explorar suas métricas, logs e rastros, independentemente
de onde estejam armazenados.
O Grafana OSS oferece ferramentas para transformar os dados do seu banco de
dados de séries temporais (TSDB) em gráficos e visualizações esclarecedores.
O framework de plugins do Grafana OSS também permite conectar outras fontes de
dados, como bancos de dados NoSQL/SQL, ferramentas de gerenciamento de tickets
como Jira ou ServiceNow e ferramentas de CI/CD como GitLab.

Após [instalar o Grafana](../setup-grafana/installation/) e configurar seu
primeiro painel seguindo as instruções em
[Começando com o Grafana](../getting-started/build-first-dashboard/), você terá
diversas opções à sua disposição, dependendo das suas necessidades.
Por exemplo, se você quiser visualizar dados meteorológicos e estatísticas sobre
sua casa inteligente, poderá criar uma
[playlist](../dashboards/create-manage-playlists/).
Se você for a pessoa administradora de uma empresa e gerenciar o Grafana para
várias equipes, poderá configurar o
[provisionamento](../administration/provisioning/) e a
[autenticação](../setup-grafana/configure-access/configure-authentication/).

As seções a seguir fornecem uma visão geral dos recursos do Grafana e links para
a documentação do produto para aprender mais sobre ele.
Para obter mais orientações e ideias, consulte nossos
[fóruns da Comunidade Grafana](https://community.grafana.com/).

## Explore métricas, logs e rastros

Explore seus dados por meio de consultas ad-hoc e detalhamento dinâmico.
Divida a visualização e compare diferentes intervalos de tempo, consultas e
fontes de dados lado a lado.
Consulte [Explore](../explore/) para obter mais informações.

## Alertas

Se você usa o Grafana Alerting, pode configurar alertas para serem enviados por
meio de diversos notificadores, incluindo PagerDuty, SMS, e-mail, VictorOps,
OpsGenie ou Slack.

Os hooks de alerta permitem criar diferentes notificadores com um pouco de
código, caso prefira outros canais de comunicação.
Defina visualmente [regras de alerta](../alerting/alerting-rules/) para suas
métricas mais importantes.

## Anotações

Anote gráficos com eventos detalhados de diferentes fontes de dados.
Passe o cursor sobre os eventos para ver os metadados e tags completos.

Este recurso, que aparece como um marcador no gráfico do Grafana, é útil para
correlacionar dados caso algo dê errado.
Você pode criar as anotações manualmente — basta clicar com o botão direito do
mouse em um gráfico e inserir algum texto — ou pode buscar dados de qualquer
fonte de dados.
Consulte [Anotações](../dashboards/build-dashboards/annotate-visualizations/)
para obter mais informações.

## Variáveis de dashboard

[Variáveis de template](../dashboards/variables/) permitem criar painéis que
podem ser reutilizados para diversos casos de uso.
Os valores não são fixos nesses templates; portanto, por exemplo, se você tiver
um servidor de produção e um servidor de teste, poderá usar o mesmo painel para
ambos.

A criação de templates permite que você explore seus dados em detalhes, por
exemplo, desde todos os dados até os dados da América do Norte, depois até os
dados do Texas e assim por diante.
Você também pode compartilhar esses painéis com outras equipes dentro da sua
organização — ou, se criar um ótimo modelo de painel para uma fonte de dados
popular, pode compartilhá-lo com toda a comunidade para que todas as pessoas
possam personalizá-lo e usá-lo.

## Configure o Grafana

Se você for uma pessoa administradora do Grafana, é importante familiarizar-se
completamente com as
[opções de configuração do Grafana](../setup-grafana/configure-grafana/) e com a
[CLI do Grafana](../cli/).

A configuração abrange tanto arquivos de configuração quanto variáveis de
ambiente.
Você pode definir portas padrão, níveis de logging, endereços IP de e-mail,
segurança e muito mais.

## Importe dashboards e plugins

Descubra centenas de [dashboards](/grafana/dashboards) e
[plugins](/grafana/plugins) na biblioteca oficial.
Graças à paixão e ao dinamismo das pessoas da comunidade, novos recursos são
adicionados semanalmente.

## Autenticação

O Grafana oferece suporte a diferentes métodos de autenticação, como LDAP e
OAuth, e permite mapear pessoas usuárias para organizações.
Consulte a
[Visão geral da autenticação de pessoas usuárias](../setup-grafana/configure-access/configure-authentication/)
para obter mais informações.

No Grafana Enterprise, você também pode mapear pessoas usuárias para equipes: se
sua empresa possui um sistema de autenticação próprio, o Grafana permite mapear
as equipes em seus sistemas internos para equipes no Grafana.
Dessa forma, você pode conceder acesso automático às equipes designadas para
cada uma delas.
Consulte o [Grafana Enterprise](grafana-enterprise/) para obter mais
informações.

## Provisionamento

Embora seja fácil clicar, arrastar e soltar para criar um único dashboard,
pessoas usuárias avançadas que precisam de vários dashboards podem optar por
automatizar a configuração com um script.
É possível criar scripts para qualquer coisa no Grafana.

Por exemplo, ao criar um novo cluster Kubernetes, você também pode criar um
Grafana automaticamente com um script que terá o servidor, o endereço IP e as
fontes de dados corretos predefinidos e bloqueados para que as pessoas usuárias
não possam alterá-los.
Essa também é uma maneira de controlar vários dashboards.
Consulte [Provisionamento](../administration/provisioning/) para obter mais
informações.

## Permissões

Quando as organizações têm uma única instalação do Grafana e várias equipes,
geralmente desejam manter os ambientes separados e, ao mesmo tempo, compartilhar
dashboards.
Você pode criar uma equipe de pessoas usuárias e definir permissões para
[pastas e dashboards](../administration/user-management/manage-dashboard-permissions/)
e até o
[nível da fonte de dados](../administration/data-source-management/#data-source-permissions)
se estiver usando o [Grafana Enterprise](grafana-enterprise/).

## Outros projetos de código aberto do Grafana Labs

Além do Grafana, o Grafana Labs também oferece os seguintes projetos de código
aberto:

**Grafana Loki:** O Grafana Loki é um conjunto de componentes de código aberto
que podem ser combinados para formar uma pilha de logging completa.
Para obter mais informações, consulte a
[documentação do Grafana Loki](/docs/loki/latest/).

**Grafana Tempo:** O Grafana Tempo é um backend de rastros distribuído de código
aberto, fácil de usar e de alto volume.
Para mais informações, consulte a
[documentação do Grafana Tempo](/docs/tempo/latest/?pg=oss-tempo&plcmt=hero-txt/).

**Grafana Mimir:** O Grafana Mimir é um projeto de software de código aberto que
fornece armazenamento escalável de longo prazo para o Prometheus.
Para mais informações sobre o Grafana Mimir, consulte a
[documentação do Grafana Mimir](/docs/mimir/latest/).

**Grafana Pyroscope:** O Grafana Pyroscope é um projeto de software de código
aberto para agregação de dados de profiling contínuo.
O profiling contínuo é um sinal de observabilidade que permite entender o uso de
recursos da sua carga de trabalho (CPU, memória, por exemplo) até o nível do
número de linha.
Para obter mais informações sobre o Grafana Pyroscope, consulte a
[documentação do Grafana Pyroscope](/docs/pyroscope/latest/).

**Grafana Faro:** O Grafana Faro é um agente JavaScript de código aberto que se
integra a aplicações web para coletar dados de monitoramento de pessoas usuárias
reais (RUM): métricas de desempenho, logs, exceções, eventos e rastros.
Para obter mais informações sobre como usar o Grafana Faro, consulte a
[documentação do Grafana Faro](/docs/grafana-cloud/monitor-applications/frontend-observability/faro-web-sdk/).

**Grafana Beyla:** O Grafana Beyla é uma ferramenta de instrumentação automática
de aplicações baseada em eBPF para observabilidade de aplicações.
O eBPF é usado para inspecionar automaticamente os executáveis da aplicação e a
camada de rede do sistema operacional, bem como capturar trechos de rastros
básicos relacionados a transações web e métricas de Rate-Errors-Duration (RED)
para serviços Linux HTTP/S e gRPC.
Toda a captura de dados ocorre sem qualquer modificação no código ou na
configuração da aplicação.
Para obter mais informações sobre o Grafana Beyla, consulte a
[documentação do Grafana Beyla](/docs/beyla/latest/).

**Grafana Alloy:** O Grafana Alloy é uma distribuição flexível, de alto
desempenho e independente de fornecedor do coletor
[OpenTelemetry](https://opentelemetry.io/) (OTel).
É totalmente compatível com os padrões de observabilidade de código aberto mais
populares, como OpenTelemetry (OTel) e Prometheus.
Para obter mais informações sobre o Grafana Alloy, consulte a
[documentação do Grafana Alloy](https://grafana.com/docs/alloy/latest/).

**Grafana k6:** O Grafana k6 é uma ferramenta de código aberto para testes de
carga que facilita e aumenta a produtividade dos testes de desempenho para
equipes de engenharia.
Para mais informações sobre o Grafana k6, consulte a
[documentação do Grafana k6](/docs/k6/latest/).

**Grafana OnCall:** O Grafana OnCall é uma ferramenta de código aberto para
gerenciamento de resposta a incidentes, criada para ajudar as equipes a
aprimorarem a colaboração e resolverem incidentes mais rapidamente.
Para mais informações sobre o Grafana OnCall, consulte a
[documentação do Grafana OnCall](/docs/oncall/latest/).
