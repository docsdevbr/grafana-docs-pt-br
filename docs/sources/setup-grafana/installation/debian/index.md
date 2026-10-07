---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/v13.2.3/docs/sources/setup-grafana/installation/debian/index.md
source_revision: a6617d3af9fc60b5b22be328e96122dc68ee4bf6
translation_status: ready

aliases:
  - ../../installation/debian/
  - ../../installation/installation/debian/
description: Guia de instalação do Grafana no Debian ou Ubuntu.
labels:
  products:
    - enterprise
    - oss
menuTitle: Debian ou Ubuntu
title: Instale o Grafana no Debian ou Ubuntu
weight: 100
---

# Instale o Grafana no Debian ou Ubuntu

Este tópico explica como instalar as dependências do Grafana, instalar o Grafana
no Linux Debian ou Ubuntu e iniciar o servidor Grafana no seu sistema Debian ou
Ubuntu.

Existem várias maneiras de instalar o Grafana: usando o repositório APT da
Grafana Labs, baixando um pacote `.deb` ou baixando um arquivo binário
`.tar.gz`.
Escolha apenas um dos métodos abaixo que melhor atenda às suas necessidades.

{{< admonition type="note" >}}
Se você realizar a instalação por meio do pacote `.deb` ou do arquivo `.tar.gz`,
deverá atualizar o Grafana manualmente a cada nova versão.
{{< /admonition >}}

O vídeo a seguir demonstra como instalar o Grafana no Debian e no Ubuntu,
conforme descrito neste documento:

{{< youtube id="_Zk_XQSjF_Q" >}}

## Instale a partir do repositório APT

Ao instalar a partir do repositório APT, o Grafana é atualizado automaticamente
quando você executa o comando `apt-get update`.

| Versão do Grafana         | Pacote             | Repositório                           |
| ------------------------- | ------------------ | ------------------------------------- |
| Grafana Enterprise        | grafana-enterprise | `https://apt.grafana.com stable main` |
| Grafana Enterprise (Beta) | grafana-enterprise | `https://apt.grafana.com beta main`   |
| Grafana OSS               | grafana            | `https://apt.grafana.com stable main` |
| Grafana OSS (Beta)        | grafana            | `https://apt.grafana.com beta main`   |

{{< admonition type="note" >}}
O Grafana Enterprise é a edição recomendada e padrão.
Ele está disponível gratuitamente e inclui todos os recursos da edição OSS.
Você também pode fazer o upgrade para o
[conjunto completo de recursos Enterprise](/products/enterprise/?utm_source=grafana-install-page),
que oferece suporte a
[plugins Enterprise](/grafana/plugins/?enterprise=1&utcm_source=grafana-install-page).
{{< /admonition >}}

Siga os passos abaixo para instalar o Grafana a partir do repositório APT:

1. Instale os pacotes de pré-requisitos:

   ```bash
   sudo apt-get install -y apt-transport-https wget gnupg
   ```

1. Importe a chave GPG:

   ```bash
   sudo mkdir -p /etc/apt/keyrings
   sudo wget -O /etc/apt/keyrings/grafana.asc https://apt.grafana.com/gpg-full.key
   sudo chmod 644 /etc/apt/keyrings/grafana.asc
   ```

1. Para adicionar um repositório para versões estáveis, execute o seguinte
   comando:

   ```bash
   echo "deb [signed-by=/etc/apt/keyrings/grafana.asc] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
   ```

1. Para adicionar um repositório para versões beta, execute o seguinte comando:

   ```bash
   echo "deb [signed-by=/etc/apt/keyrings/grafana.asc] https://apt.grafana.com beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
   ```

1. Execute o seguinte comando para atualizar a lista de pacotes disponíveis:

   ```bash
   # Atualiza a lista de pacotes disponíveis
   sudo apt-get update
   ```

1. Para instalar o Grafana OSS, execute o seguinte comando:

   ```bash
   # Instala a versão OSS mais recente:
   sudo apt-get install grafana
   ```

1. Para instalar o Grafana Enterprise, execute o seguinte comando:

   ```bash
   # Instala a versão Enterprise mais recente:
   sudo apt-get install grafana-enterprise
   ```

## Instale o Grafana usando um pacote deb

Se você instalar o Grafana usando manualmente o pacote deb, deverá atualizar o
Grafana manualmente a cada nova versão.

Siga estes passos para instalar o Grafana usando um pacote deb:

1. Acesse a [página de download do Grafana](/grafana/download).
1. Selecione a versão do Grafana que deseja instalar.
   - A versão mais recente do Grafana é selecionada por padrão.
   - O campo **Version** exibe apenas versões com tag.
     Se quiser instalar uma versão diária de desenvolvimento, clique em
     **Nightly Builds** e selecione uma versão.
1. Selecione uma **Edition**.
   - **Enterprise:** esta é a versão recomendada.
     Ela é funcionalmente idêntica à versão de código aberto, mas inclui
     recursos que podem ser desbloqueados com uma licença, caso você opte por
     isso.
   - **Open Source:** esta versão é funcionalmente idêntica à versão Enterprise,
     mas você precisará baixar a versão Enterprise se quiser utilizar os
     recursos Enterprise.
1. Dependendo do sistema que você está utilizando, clique na aba **Linux** ou
   **ARM** na [página de download](/grafana/download).
1. Copie e cole o código da [página de download](/grafana/download) na sua linha
   de comando e execute-o.

## Instale o Grafana como um binário independente

Siga os passos abaixo para instalar o Grafana usando os binários independentes:

1. Acesse a [página de download do Grafana](/grafana/download).
1. Selecione a versão do Grafana que deseja instalar.
   - A versão mais recente do Grafana é selecionada por padrão.
   - O campo **Version** exibe apenas versões com tag.
     Se quiser instalar uma versão diária de desenvolvimento, clique em
     **Nightly Builds** e selecione uma versão.
1. Selecione uma **Edition**.
   - **Enterprise:** esta é a versão recomendada.
     Ela é funcionalmente idêntica à versão de código aberto, mas inclui
     recursos que podem ser desbloqueados com uma licença, caso você opte por
     isso.
   - **Open Source:** esta versão é funcionalmente idêntica à versão Enterprise,
     mas você precisará baixar a versão Enterprise se quiser utilizar os
     recursos Enterprise.
1. Dependendo do sistema que você está utilizando, clique na aba **Linux** ou
   **ARM** na [página de download](/grafana/download).
1. Copie e cole o código da [página de download](/grafana/download) na sua linha
   de comando e execute-o.
1. Crie uma conta de usuário para o Grafana no seu sistema:

   ```shell
   sudo useradd -r -s /bin/false grafana
   ```

1. Mova o binário descompactado para `/usr/local/grafana`:

   ```shell
   sudo mv <DOWNLOAD PATH> /usr/local/grafana
   ```

1. Altere o proprietário de `/usr/local/grafana` para os usuários do Grafana:

   ```shell
   sudo chown -R grafana:users /usr/local/grafana
   ```

1. Crie um arquivo de unidade systemd para o servidor Grafana:

   ```shell
   sudo touch /etc/systemd/system/grafana-server.service
   ```

1. Adicione o seguinte ao arquivo de unidade em um editor de texto de sua
   escolha:

   ```ini
   [Unit]
   Description=Grafana Server
   After=network.target

   [Service]
   Type=simple
   User=grafana
   Group=users
   ExecStart=/usr/local/grafana/bin/grafana server --config=/usr/local/grafana/conf/grafana.ini --homepath=/usr/local/grafana
   Restart=on-failure

   [Install]
   WantedBy=multi-user.target
   ```

1. Use o binário para iniciar manualmente o servidor Grafana:

   ```shell
   /usr/local/grafana/bin/grafana server --homepath /usr/local/grafana
   ```

   {{< admonition type="note" >}}
   A execução manual do binário nesta etapa cria automaticamente o diretório
   `/usr/local/grafana/data`, o qual precisa ser criado e configurado antes que
   a instalação possa ser considerada concluída.
   {{< /admonition >}}

1. Pressione `CTRL+C` para parar o servidor Grafana.
1. Altere novamente o proprietário de `/usr/local/grafana` para os usuários do
   Grafana, a fim de aplicar a propriedade ao diretório
   `/usr/local/grafana/data` recém-criado:

   ```shell
   sudo chown -R grafana:users /usr/local/grafana
   ```

1. [Configure o servidor Grafana para iniciar na inicialização do sistema usando o systemd](https://grafana.com/docs/grafana/latest/setup-grafana/start-restart-grafana/#configure-the-grafana-server-to-start-at-boot-using-systemd).

## Desinstalação no Debian ou Ubuntu

Realize qualquer um dos passos a seguir para desinstalar o Grafana.

Para desinstalar o Grafana, execute os seguintes comandos em uma janela de
terminal:

1. Se você configurou o Grafana para ser executado com o systemd, pare o serviço
   do systemd para o servidor Grafana:

   ```shell
   sudo systemctl stop grafana-server
   ```

1. Se você configurou o Grafana para ser executado com o init.d, pare o serviço
   do init.d para o servidor Grafana:

   ```shell
   sudo service grafana-server stop
   ```

1. Para desinstalar o Grafana OSS:

   ```shell
   sudo apt-get remove grafana
   ```

1. Para desinstalar o Grafana Enterprise:

   ```shell
   sudo apt-get remove grafana-enterprise
   ```

1. Opcional: para remover o repositório do Grafana:

   ```bash
   sudo rm -i /etc/apt/sources.list.d/grafana.list
   ```

## Próximos passos

- [Inicie o servidor Grafana](../../start-restart-grafana/)
