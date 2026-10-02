---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/getting-started/first-dashboards/_index.md
source_revision: 34c3bb0104bfe4cf94b390e855eb88d5bd987a78
translation_status: ready

aliases:
  - ../../guides/getting_started/ # /docs/grafana/latest/guides/getting_started/
  - ../../guides/gettingstarted/ # /docs/grafana/latest/guides/gettingstarted/
  - ../../getting-started/build-first-dashboard/ # /docs/grafana/latest/getting-started/build-first-dashboard/
description: >-
  Aprenda como começar a usar o Grafana adicionando um dashboard
  pré-configurado.
labels:
  products:
    - enterprise
    - oss
title: Crie seu primeiro dashboard
weight: 200
---

# Crie seu primeiro dashboard

Este tópico ajuda você a começar a usar o Grafana e a criar seu primeiro
dashboard usando a fonte de dados integrada `Grafana`.
Para saber mais sobre o Grafana, consulte a
[Introdução ao Grafana](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/introduction/).

{{< admonition type="note" >}}
O Grafana também oferece uma
[conta gratuita no Grafana Cloud](/signup/cloud/connect-account?pg=gsdocs) para
ajudar você a começar de forma ainda mais fácil e rápida.
Você pode instalar o Grafana em seu próprio servidor ou obter uma conta gratuita
no Grafana Cloud.
{{< /admonition >}}

#### Instale o Grafana

O Grafana pode ser instalado em diversos sistemas operacionais.
Para obter uma lista dos requisitos mínimos de hardware e software, bem como
instruções de instalação, consulte
[Instale o Grafana](https://grafana.com/docs/grafana/<VERSÃO_DO_GRAFANA>/setup-grafana/installation/).

#### Entre no Grafana

Para entrar no Grafana pela primeira vez:

1. Abra seu navegador e acesse http://localhost:3000/.

   A porta HTTP padrão que o Grafana utiliza é a `3000`, a menos que você tenha
   configurado uma porta diferente.

1. Na página de login, digite `admin` como nome de usuário e senha.

1. Clique em **Sign in**.

   Se o login for bem-sucedido, você verá uma mensagem solicitando a alteração
   da senha.

1. Clique em **OK** na mensagem e altere sua senha.

{{< admonition type="note" >}}
Recomendamos fortemente que você altere a senha padrão do usuário administrador.
{{< /admonition >}}

#### Crie um dashboard

Se você já configurou uma fonte de dados que sabe como consultar, consulte
[Crie um dashboard](https://grafana.com/docs/grafana/<VERSÃO_DO_GRAFANA>/dashboards/build-dashboards/create-dashboard/).

Para criar seu primeiro dashboard usando a fonte de dados integrada
`-- Grafana --`:

1. Clique em **Dashboards** no menu principal.
1. Na página **Dashboards**, clique em **New** e selecione **New Dashboard** no
   menu suspenso.
1. Em **Add** no painel de edição, clique ou arraste um painel para o dashboard.

   {{< figure src="/media/docs/grafana/dashboards/screenshot-add-panel-v12.4.png" max-width="750px" alt="Novo dashboard" >}}

1. No novo painel, clique em **Configure**.

   A visualização **Edit panel** será aberta com a fonte de dados padrão da sua
   instância pré-selecionada.

1. Na guia **Queries**, clique na lista suspensa **Data source**, digite
   `-- Grafana --` e selecione essa fonte de dados.

   Isso configura sua
   [consulta](https://grafana.com/docs/grafana/<VERSÃO_DO_GRAFANA>/panels-visualizations/query-transform-data/#add-a-query)
   e gera o dashboard Random Walk.

1. No painel **Edit pane**, selecione a visualização **Time series**.
1. Clique em **Refresh** para consultar a fonte de dados.
1. Ao terminar de editar o painel, clique em **Save dashboard**.

   Como alternativa, clique em **Back to dashboard** se quiser ver as alterações
   aplicadas ao dashboard primeiro.
   Em seguida, clique em **Save dashboard** quando tiver terminado.

1. Adicione um título descritivo para o dashboard ou peça ao Grafana para criar
   um usando
   [recursos de IA generativa](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/dashboards/manage-dashboards#set-up-generative-ai-features-for-dashboards)
   e clique em **Save**.
1. Clique em **Back to dashboard** e depois em **Exit edit**.

Parabéns, você criou seu primeiro dashboard e ele está exibindo resultados.

#### Próximos passos

Continue a experimentar com o que você criou, tente o
[fluxo de trabalho do Explore](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/explore/)
ou outro recurso de visualização.
Consulte [Fontes de dados](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/datasources/)
para obter uma lista das fontes de dados suportadas e instruções sobre como
[adicionar uma fonte de dados](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/datasources/#add-a-data-source).
Os seguintes tópicos serão do seu interesse:

- [Painéis e visualizações](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/panels-visualizations/)
- [Dashboards](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/dashboards/)
- [Atalhos de teclado](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/dashboards/use-dashboards/#keyboard-shortcuts)
- [Plugins](/grafana/plugins?orderBy=weight&direction=asc)

##### Pessoas administradoras

Os seguintes tópicos são de interesse para pessoas usuárias administradoras do
servidor Grafana:

- [Configuração do Grafana](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-grafana/)
- [Autenticação](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-access/configure-authentication/)
- [Permissões e funções de usuário](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/administration/roles-and-permissions/)
- [Provisionamento](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/administration/provisioning/)
- [CLI do Grafana](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/cli/)
