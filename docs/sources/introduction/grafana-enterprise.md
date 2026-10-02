---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/v13.2.3/docs/sources/introduction/grafana-enterprise.md
source_revision: ba6a783997552ad5154918918f79ba1ab06584bc
translation_status: ready

aliases:
  - ../enterprise/
description: Visão geral do Grafana Enterprise
labels:
  products:
    - enterprise
title: Grafana Enterprise
weight: 200
---

# Grafana Enterprise

O Grafana Enterprise é uma edição comercial do Grafana que inclui recursos
adicionais não encontrados na versão de código aberto.

Baseado em tudo o que você já conhece e ama no Grafana de código aberto, o
Grafana Enterprise inclui
[plugins exclusivos de fontes de dados](#enterprise-data-sources) e
[recursos adicionais](#enterprise-features).
Você também recebe suporte 24x7x365 e treinamento do time principal do Grafana.

Para saber mais sobre o Grafana Enterprise, consulte
[nossa página do produto](/enterprise).

## Recursos do Grafana Enterprise no Grafana Cloud

Muitos recursos do Grafana Enterprise também estão disponíveis em contas pagas
do [Grafana Cloud](/docs/grafana-cloud).
Para obter detalhes, consulte os
[recursos do Grafana Cloud](/docs/grafana-cloud/introduction/understand-grafana-cloud-features/).
Para obter informações sobre preços e planos, consulte os
[preços do Grafana Cloud](https://grafana.com/pricing/).

Para migrar para o Grafana Cloud, consulte
[Migre do Grafana Enterprise para o Grafana Cloud](/docs/grafana/<GRAFANA_VERSION>/administration/migration-guide/).

## Autenticação

O Grafana Enterprise inclui integrações com mais maneiras de autenticar suas
pessoas usuárias e recursos de autenticação aprimorados.

### Sincronização de equipes

A
[Sincronização de equipes](/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-team-sync/)
permite configurar a sincronização entre equipes no Grafana e equipes no seu
provedor de autenticação, para que suas pessoas usuárias sejam automaticamente
alocadas à equipe correta.

Provedores de autenticação compatíveis:

- [Auth Proxy](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/auth-proxy#team-sync-enterprise-only)
- [Entra ID OAuth](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/azuread/#team-sync-enterprise-only)
- [GitHub OAuth](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/github/#configure-team-synchronization)
- [Generic OAuth integration](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/generic-oauth/#configure-team-synchronization)
- [GitLab OAuth](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/gitlab/#configure-team-synchronization)
- [Google OAuth](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/google/#configure-team-synchronization)
- [LDAP](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/enhanced-ldap/#ldap-group-synchronization-for-teams)
- [Okta](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/okta#configure-team-synchronization-enterprise-only)
- [SAML](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/saml#configure-team-sync)

### Integração LDAP aprimorada

Com a
[integração LDAP aprimorada](/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/enhanced-ldap/),
você pode configurar a sincronização LDAP ativa.

### Autenticação SAML

A
[autenticação SAML](/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/saml/)
permite que as pessoas usuárias se autentiquem com serviços de logon único que
utilizam a Linguagem de Marcação de Asserção de Segurança (SAML).

### Funções protegidas

Com as
[funções protegidas](/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/#protected-roles),
você pode definir funções de pessoas usuárias que não serão convertidas de um
tipo de autenticação para outro ao trocar de provedor de autenticação.

## Recursos da versão Enterprise

O Grafana Enterprise adiciona os seguintes recursos:

- [Controle de acesso baseado em funções](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/administration/roles-and-permissions/access-control/)
  para controlar o acesso com permissões baseadas em funções.
- [Permissões da fonte de dados](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/administration/data-source-management/#data-source-permissions)
  para restringir o acesso a consultas a equipes e pessoas usuárias específicas.
- [Cache de consultas e recursos da fonte de dados](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/administration/data-source-management/#query-and-resource-caching)
  para armazenar temporariamente os resultados das consultas no Grafana,
  reduzindo a carga da fonte de dados e a limitação de taxa.
- [Relatórios](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/dashboards/create-reports/)
  para gerar um relatório em PDF a partir de qualquer dashboard e configurar um
  agendamento para enviá-lo por e-mail para quem você escolher.
- [Exportar dashboard como PDF](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/dashboards/share-dashboards-panels/#export-a-dashboard-as-pdf)
- [Personalização da marca](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-grafana/configure-custom-branding/)
  para personalizar o Grafana, desde a marca e o logotipo até os links do
  rodapé.
- [Informações de uso](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/dashboards/assess-dashboard-usage/)
  para entender como sua instância do Grafana está sendo usada.
- [Consultas gravadas](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/administration/recorded-queries/)
  para visualizar tendências ao longo do tempo para suas fontes de dados.
- [Integração com o Vault](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-security/configure-database-encryption/#encrypting-your-database-with-a-key-from-a-key-management-service-kms)
  para gerenciar seus segredos de configuração ou provisionamento com o Vault.
- A
  [Auditoria](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-security/audit-grafana/)
  rastreia alterações importantes na sua instância do Grafana para ajudar você a
  gerenciar e mitigar atividades suspeitas e atender aos requisitos de
  conformidade.
- A
  [Segurança de requisição](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-security/configure-request-security/)
  permite restringir as requisições de saída do servidor Grafana.
- A
  [Atualização de configurações em tempo de execução](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-grafana/settings-updates-at-runtime/)
  permite atualizar as configurações do Grafana em tempo de execução sem a
  necessidade de reiniciá-lo.

## Fontes de dados corporativas

Com uma licença Grafana Enterprise, você também obtém acesso a fontes de dados
premium, incluindo:

- [Adobe Analytics](/grafana/plugins/grafana-adobeanalytics-datasource)
- [Amazon Aurora](/grafana/plugins/grafana-aurora-datasource)
- [AppDynamics](/grafana/plugins/dlopes7-appdynamics-datasource)
- [Atlassian Statuspage](/grafana/plugins/grafana-atlassianstatuspage-datasource)
- [Azure CosmosDB](/grafana/plugins/grafana-azurecosmosdb-datasource)
- [Azure Devops](/grafana/plugins/grafana-azuredevops-datasource)
- [Catchpoint](/grafana/plugins/grafana-catchpoint-datasource)
- [Cloudflare](/grafana/plugins/grafana-cloudflare-datasource)
- [CockroachDB](/grafana/plugins/grafana-cockroachdb-datasource)
- [Databricks](/grafana/plugins/grafana-databricks-datasource)
- [DataDog](/grafana/plugins/grafana-datadog-datasource)
- [IBM Db2](/grafana/plugins/grafana-ibmdb2-datasource)
- [Drone](/grafana/plugins/grafana-drone-datasource)
- [DynamoDB](/grafana/plugins/grafana-dynamodb-datasource/)
- [Dynatrace](/grafana/plugins/grafana-dynatrace-datasource)
- [Gitlab](/grafana/plugins/grafana-gitlab-datasource)
- [Grafana Enterprise Logs](/grafana/plugins/grafana-enterprise-logs-app/)
- [Grafana Enterprise Metrics](/grafana/plugins/grafana-metrics-enterprise-app/)
- [Grafana Enterprise Traces](/grafana/plugins/grafana-enterprise-traces-app/)
- [Honeycomb](/grafana/plugins/grafana-honeycomb-datasource)
- [Jira](/grafana/plugins/grafana-jira-datasource)
- [LogicMonitor Devices](/grafana/plugins/grafana-logicmonitor-datasource/)
- [Looker](/grafana/plugins/grafana-looker-datasource/)
- [MongoDB](/grafana/plugins/grafana-mongodb-datasource)
- [Netlify](/grafana/plugins/grafana-netlify-datasource)
- [New Relic](/grafana/plugins/grafana-newrelic-datasource)
- [Oracle Database](/grafana/plugins/grafana-oracle-datasource)
- [PagerDuty](/grafana/plugins/grafana-pagerduty-datasource)
- [Salesforce](/grafana/plugins/grafana-salesforce-datasource)
- [SAP HANA®](/grafana/plugins/grafana-saphana-datasource)
- [ServiceNow](/grafana/plugins/grafana-servicenow-datasource)
- [Snowflake](/grafana/plugins/grafana-snowflake-datasource)
- [SolarWinds](/grafana/plugins/grafana-solarwinds-datasource)
- [Splunk](/grafana/plugins/grafana-splunk-datasource)
- [Splunk Infrastructure monitoring (SignalFx)](/grafana/plugins/grafana-splunk-monitoring-datasource)
- [Sqlyze Datasource](/grafana/plugins/grafana-odbc-datasource)
- [SumoLogic](/grafana/plugins/grafana-sumologic-datasource)
- [Wavefront](/grafana/plugins/grafana-wavefront-datasource)
- [Zendesk](/grafana/plugins/grafana-zendesk-datasource)

## Experimente o Grafana Enterprise

Para comprar ou obter uma licença de avaliação, entre em contato com a
[Equipe de Vendas](/contact?about=grafana-enterprise-stack) da Grafana Labs.
