---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/getting-started/first-dashboards/get-started-grafana-ms-sql-server.md
source_revision: 83e190a160cc4ea88e606b9d43b09d4ab8ceb1fa
translation_status: ready

aliases:
  - ../../../getting-started/getting-started-sql/ # /docs/grafana/latest/getting-started/getting-started-sql
  - ../../../getting-started/get-started-grafana-ms-sql-server/ # /docs/grafana/latest/getting-started/get-started-grafana-ms-sql-server
description: Aprenda a criar seu primeiro dashboard do MS SQL Server no Grafana.
labels:
  products:
    - enterprise
    - oss
title: Comece a usar o Grafana e o MS SQL Server
weight: 500
---

# Comece a usar o Grafana e o MS SQL Server

O Microsoft SQL Server é um sistema de gerenciamento de banco de dados
relacional popular e amplamente utilizado em ambientes de desenvolvimento e
produção.
Este tópico orienta você nos passos para criar uma série de dashboards no
Grafana para exibir métricas de um banco de dados MS SQL Server.

#### Baixe o MS SQL Server

O MS SQL Server pode ser instalado em sistemas operacionais Windows ou Linux,
bem como em contêineres Docker.
Consulte a
[página de downloads do MS SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
para obter uma lista completa de todas as opções disponíveis.

#### Instale o MS SQL Server

Você pode instalar o MS SQL Server no host que executa o Grafana ou em um
servidor remoto.
Para instalar o software a partir da
[página de downloads](https://www.microsoft.com/en-us/sql-server/sql-server-downloads),
siga as instruções de instalação.

Se você estiver em um host Windows, mas quiser usar o Grafana e a fonte de dados
MS SQL em um ambiente Linux, consulte a postagem do blog
[WSL para configurar seu ambiente de desenvolvimento do Grafana](/blog/2021/03/03/how-to-set-up-a-grafana-development-environment-on-a-windows-pc-using-wsl).
Isso permitirá que você aproveite os recursos disponíveis no repositório GitHub
[grafana/grafana](https://github.com/grafana/grafana).
Lá você encontrará uma coleção de fontes de dados compatíveis, incluindo o MS
SQL Server, juntamente com dados de teste e dashboards pré-configurados para
uso.

#### Adicione a fonte de dados MS SQL

Existem várias maneiras de autenticar no MSSQL. Comece por:

1. Clique em **Connections** no menu à esquerda e filtre por `mssql`.
1. Selecione a opção **Microsoft SQL Server**.
1. Clique em **Create a Microsoft SQL Server data source** no canto superior
   direito para abrir a página de configuração.
1. Selecione o método de autenticação desejado e preencha as informações
   corretas, conforme detalhado abaixo.
1. Clique em **Save & test**.

##### Configuração geral

| Nome       | Descrição                                                                                                                           |
|------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `Name`     | O nome da fonte de dados. É como você se refere à fonte de dados em painéis e consultas.                                            |
| `Host`     | O endereço IP/nome do host e a porta (opcional) da sua instância do MS SQL. Se a porta for omitida, a porta padrão 1433 será usada. |
| `Database` | Nome do seu banco de dados MS SQL.                                                                                                  |

##### Autenticação do SQL Server

| Nome       | Descrição                                                  |
|------------|------------------------------------------------------------|
| `User`     | Login/nome de usuário da pessoa usuária do banco de dados. |
| `Password` | Senha do usuário do banco de dados.                        |

#### Windows Active Directory (Kerberos)

Abaixo estão as quatro maneiras possíveis de autenticar via Windows Active
Directory/Kerberos.

{{< admonition type="note" >}}
A autenticação do Windows Active Directory (Kerberos) não é suportada no Grafana
Cloud no momento.
{{< /admonition >}}

| Método                    | Descrição                                                                                                                                                                            |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Username + password**   | Insira o usuário e a senha do domínio.                                                                                                                                               |
| **Keytab file**           | Especifique o caminho para um arquivo keytab válido para usar na autenticação.                                                                                                       |
| **Credential cache**      | Faça login no host via `kinit` e passe o caminho para o cache de credenciais. O caminho do cache pode ser encontrado executando `klist` no host em questão.                          |
| **Credential cache file** | Esta opção permite que várias configurações válidas estejam presentes e a correspondência é realizada no host, banco de dados e usuário. Veja o exemplo de JSON abaixo desta tabela. |

```json
[
  {
    "user": "grot@GF.LAB",
    "database": "dbone",
    "address": "mysql1.mydomain.com:3306",
    "credentialCache": "/tmp/krb5cc_1000"
  },
  {
    "user": "grot@GF.LAB",
    "database": "dbtwo",
    "address": "mysql2.gf.lab",
    "credentialCache": "/tmp/krb5cc_1000"
  }
]
```

Para instalações a partir do repositório
[grafana/grafana](https://github.com/grafana/grafana/tree/main), a fonte de
dados `gdev-mssql` está disponível.
Após adicionar essa fonte de dados, você pode usar o painel
`Datasource tests - MSSQL` com três painéis que exibem métricas geradas a partir
de um banco de dados de teste.

![Dashboard do MS SQL Server](/static/img/docs/getting-started/gdev-sql-dashboard.png)

Opcionalmente, explore este dashboard e personalize-o para:

- Criar painéis diferentes.
- Alterar os títulos dos painéis.
- Alterar a frequência de coleta de dados.
- Alterar o período de exibição dos dados.
- Reorganizar e redimensionar os painéis.

#### Comece a criar dashboards

Agora que você já tem uma ideia de como usar a fonte de dados MS SQL
pré-configurada e alguns dados de teste, o próximo passo é configurar sua
própria instância do banco de dados MS SQL Server e seus dados para o ambiente
de desenvolvimento ou sandbox.

Para obter dados da sua própria instância do MS SQL Server, adicione a fonte de
dados seguindo as instruções da Etapa 4 deste tópico.
No Grafana [Explore](../../explore/), crie consultas para experimentar as
métricas que deseja monitorar.

Depois de ter uma lista selecionada de consultas, crie
[dashboards](../../dashboards/) para exibir as métricas do banco de dados SQL
Server.
Para solução de problemas, permissões de usuário, problemas conhecidos e
exemplos de consultas, consulte
[Usando o Microsoft SQL Server no Grafana](../../datasources/mssql/).
