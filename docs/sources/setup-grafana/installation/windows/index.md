---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/v13.2.3/docs/sources/setup-grafana/installation/windows/index.md
source_revision: 13cf67de539c7e0de938818ab26df25c6681e1f3
translation_status: ready

aliases:
  - ../../installation/windows/
description: Como instalar o Grafana OSS ou Enterprise no Windows
labels:
  products:
    - enterprise
    - oss
menuTitle: Windows
title: Instalar o Grafana no Windows
weight: 700
---

# Instalar o Grafana no Windows

O vídeo a seguir demonstra como instalar o Grafana usando o instalador
independente para Windows, conforme descrito neste documento:

{{< youtube id="js2bZijbhJM" >}}

Você pode instalar o Grafana usando o instalador para Windows ou o arquivo
binário independente para Windows.

1. Acesse a [página de download do Grafana](/grafana/download).
1. Selecione a versão do Grafana que deseja instalar.
  - A versão mais recente do Grafana é selecionada por padrão.
  - O campo **Version** exibe apenas versões com tag.
  - Se quiser instalar uma versão de desenvolvimento diária, clique em **Nightly
    Builds** e selecione uma versão.
1. Selecione uma **Edition**.
  - **Enterprise:** esta é a versão recomendada.
    Ela é funcionalmente idêntica à versão de código aberto, mas inclui recursos
    que podem ser desbloqueados com uma licença, caso você opte por isso.
  - **Open Source:** esta versão é funcionalmente idêntica à versão Enterprise,
    mas você precisará baixar a versão Enterprise se quiser utilizar os recursos
    Enterprise.
1. Clique em **Windows**.
2. Para usar o instalador do Windows, siga estes passos:

   a. Clique em **Download the installer** (Baixar o instalador).

   b. Abra e execute o instalador.

3. Para instalar o binário independente para Windows, siga estes passos:

   a. Clique em **Download the zip file**.

   b. Clique com o botão direito no arquivo baixado, selecione **Propriedades**,
      marque a caixa de seleção `unblock` e clique em `OK`.

   c. Extraia o arquivo ZIP para qualquer pasta.

Inicie o Grafana executando o arquivo `grafana-server.exe`, localizado no
diretório `bin`, preferencialmente via linha de comando.
Se quiser executar o Grafana como um serviço do Windows, baixe o
[NSSM](https://nssm.cc/).
É muito fácil adicionar o Grafana como um serviço do Windows usando essa
ferramenta.

1. Para executar o Grafana, abra seu navegador e acesse a porta do Grafana (o
   padrão é http://localhost:3000/) e, em seguida, siga as instruções em
   [Getting Started](../../../getting-started/build-first-dashboard/).

   > **Nota:** A porta padrão do Grafana é `3000`.
   > Essa porta pode exigir permissões adicionais no Windows.
   > Se o Grafana não aparecer na porta padrão, você pode alterar o número da
   > porta.

2. Para alterar a porta, siga estes passos:

   a. Abra o diretório `conf` e copie o arquivo `sample.ini` para `custom.ini`.

      > **Nota:** Você deve editar o `custom.ini`, nunca o `defaults.ini`.

   b. Edite o `custom.ini` e remova o comentário da opção de configuração
      `http_port`.

      O caractere `;` é usado para comentários em arquivos `.ini`.

   c. Altere a porta para `8080` ou algo semelhante.

      A porta `8080` não deve exigir privilégios adicionais no Windows.

## Próximos passos

- [Inicie o servidor Grafana](../../start-restart-grafana/)
