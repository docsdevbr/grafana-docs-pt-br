---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/v13.2.3/docs/sources/setup-grafana/installation/mac/index.md
source_revision: c94f930950e026e79424f9621f8b6f01a8e3e254
translation_status: ready

aliases:
  - ../../installation/mac/
description: Como instalar o Grafana OSS ou Enterprise no macOS.
labels:
  products:
    - enterprise
    - oss
menuTitle: macOS
title: Instale o Grafana no macOS
weight: 600
---

# Instale o Grafana no macOS

Esta página explica como instalar o Grafana no macOS.

O vídeo a seguir demonstra como instalar o Grafana no macOS, conforme descrito
neste documento:

{{< youtube id="1zdm8SxOLYQ" >}}

## Instale o Grafana no macOS usando Homebrew

Para instalar o Grafana no macOS usando o Homebrew, siga estes passos:

1. Na página inicial do [Homebrew](http://brew.sh/), pesquise por Grafana.

   A última versão estável lançada será exibida.

1. Abra um terminal e execute os seguintes comandos:

   ```
   brew update
   brew install grafana
   ```

   O Homebrew baixa e extrai os arquivos para:

   - `/usr/local/Cellar/grafana/[versão]` (Intel Silicon)
   - `/opt/homebrew/Cellar/grafana/[versão]` (Apple Silicon)

1. Para iniciar o Grafana, execute o seguinte comando:

   ```bash
   brew services start grafana
   ```

### Usando a CLI do Grafana com o Homebrew

Para usar a CLI do Grafana com o Homebrew, você precisa adicionar o caminho
inicial, o caminho do arquivo de configuração e — dependendo do comando —
algumas outras configurações ao comando `cli`:

Para comandos `admin`, você precisa adicionar a configuração
`--configOverrides cfg:default.paths.data=/opt/homebrew/var/lib/grafana`.
Exemplo:

```bash
/opt/homebrew/opt/grafana/bin/grafana cli --config /opt/homebrew/etc/grafana/grafana.ini --homepath /opt/homebrew/opt/grafana/share/grafana --configOverrides cfg:default.paths.data=/opt/homebrew/var/lib/grafana admin reset-admin-password <new password>
```

Para comandos `plugins`, você precisa adicionar a configuração
`--pluginsDir /opt/homebrew/var/lib/grafana/plugins`.
Exemplo:

```bash
/opt/homebrew/opt/grafana/bin/grafana cli --config /opt/homebrew/etc/grafana/grafana.ini --homepath /opt/homebrew/opt/grafana/share/grafana --pluginsDir "/opt/homebrew/var/lib/grafana/plugins" plugins install <plugin-id>
```

## Instale binários independentes para macOS

Para instalar o Grafana no macOS usando os binários independentes, siga estes
passos:

1. Acesse a [página de download do Grafana](/grafana/download).
1. Selecione a versão do Grafana que você deseja instalar.
   - A versão mais recente do Grafana é selecionada por padrão.
   - O campo **Version** exibe apenas versões com tag.
     Se você quiser instalar uma versão de desenvolvimento diária, clique em
     **Nightly Builds** e selecione uma versão.
1. Selecione uma **Edition**.
   - **Enterprise:** esta é a versão recomendada.
     Ela é funcionalmente idêntica à versão de código aberto, mas inclui
     recursos que podem ser desbloqueados com uma licença, caso você opte por
     isso.
   - **Open Source:** esta versão é funcionalmente idêntica à versão Enterprise,
     mas você precisará baixar a versão Enterprise se quiser utilizar os
     recursos Enterprise.
1. Clique em **Mac**.
1. Copie e cole o código da [página de download](/grafana/download) na sua linha
   de comando e execute-o.
1. Extraia o arquivo `gz` e copie os arquivos para o local de sua preferência.
1. Para iniciar o serviço do Grafana, vá para o diretório e execute o comando:

   ```bash
   ./bin/grafana server
   ```

Alternativamente, assista ao vídeo "Grafana para iniciantes" abaixo:

{{< youtube id="T51Qa7eE3W8" >}}

## Próximos passos

- [Inicie o servidor Grafana](../../start-restart-grafana/)
