---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/v13.2.3/docs/sources/setup-grafana/installation/docker/index.md
source_revision: 79596aa922f3d132330fe1579d7f8158021c7f70
translation_status: ready

aliases:
  - ../../installation/docker/
description: Guia para executar o Grafana usando o Docker.
labels:
  products:
    - enterprise
    - oss
menuTitle: Imagem Docker do Grafana
title: Execute a imagem Docker do Grafana
weight: 400
---

{{< admonition type="caution" >}}
A partir da versão `12.4.0` do Grafana, o repositório `grafana/grafana-oss` no
Docker Hub não será mais atualizado.
Em vez disso, recomendamos utilizar o repositório `grafana/grafana` no Docker
Hub.
Ambos os repositórios contêm as mesmas imagens Docker do Grafana OSS.
{{< /admonition >}}

# Execute a imagem Docker do Grafana

Este tópico orienta você na instalação do Grafana utilizando as imagens Docker
oficiais.
Especificamente, aborda a execução do Grafana por meio da interface de linha de
comando (CLI) do Docker e do docker-compose.

{{< youtube id="FlDfcMbSLXs" start="703">}}

As imagens Docker do Grafana estão disponíveis em duas edições:

- **Grafana Enterprise**: `grafana/grafana-enterprise`
- **Grafana Open Source**: `grafana/grafana`

> **Nota:** A edição recomendada e padrão do Grafana é o Grafana Enterprise.
> Ele é gratuito e inclui todos os recursos da edição OSS.
> Além disso, você tem a opção de fazer upgrade para o
> [conjunto completo de recursos Enterprise](/products/enterprise/?utm_source=grafana-install-page),
> que inclui suporte a
> [plugins Enterprise](/grafana/plugins/?enterprise=1&utcm_source=grafana-install-page).

As imagens padrão do Grafana são criadas com base no projeto Alpine Linux e
podem ser encontradas na imagem oficial do Alpine.
Para obter instruções sobre como configurar uma imagem Docker para o Grafana,
consulte [Configure uma imagem Docker do Grafana](../../configure-docker/).

## Execute o Grafana usando a CLI do Docker

Esta seção mostra como executar o Grafana usando a CLI do Docker.

> **Nota:** Se você estiver em um sistema Linux (por exemplo, Debian ou Ubuntu),
> talvez precise adicionar `sudo` antes do comando ou adicionar seu usuário ao
> grupo `docker`.
> Para mais informações, consulte os
> [Passos da pós-instalação da Docker Engine no Linux](https://docs.docker.com/engine/install/linux-postinstall/).

Para executar a versão estável mais recente do Grafana, execute o seguinte
comando:

```bash
docker run -d -p 3000:3000 --name=grafana grafana/grafana-enterprise
```

Onde:

- [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) é um
  comando da CLI do Docker que executa um novo contêiner a partir de uma imagem.
- `-d` (`--detach`) executa o contêiner em segundo plano.
- `-p <porta-do-host>:<porta-do-contêiner>` (`--publish`) publica as portas do
  contêiner no host, permitindo acessar a porta do contêiner por meio de uma
  porta do host.
  Neste caso, podemos acessar a porta `3000` do contêiner através da porta
  `3000` do host.
- `--name` atribui um nome lógico ao contêiner (por exemplo, `grafana`).
  Isso permite referenciar o contêiner pelo nome em vez de pelo ID.
- `grafana/grafana-enterprise` é a imagem a ser executada.

### Pare o contêiner do Grafana

Para parar o contêiner do Grafana, execute o seguinte comando:

```bash
# O comando `docker ps` mostra os processos em execução no Docker
docker ps

# Isso exibirá uma lista de contêineres semelhante a esta:
CONTAINER ID   IMAGE  COMMAND   CREATED  STATUS   PORTS    NAMES
cd48d3994968   grafana/grafana-enterprise   "/run.sh"   8 seconds ago   Up 7 seconds   0.0.0.0:3000->3000/tcp   grafana

# Para parar o contêiner do Grafana, execute o comando docker stop CONTAINER-ID
# ou use docker stop NAME, que é `grafana`, conforme definido anteriormente.
docker stop grafana
```

### Salve seus dados do Grafana

Por padrão, o Grafana utiliza um banco de dados SQLite versão 3 embutido para
armazenar configurações, usuários, dashboards e outros dados.
Ao executar imagens do Docker como contêineres, as alterações nesses dados do
Grafana são gravadas no sistema de arquivos dentro do contêiner, persistindo
apenas enquanto o contêiner existir.
Se você parar e remover o contêiner, quaisquer alterações no sistema de arquivos
(ou seja, os dados do Grafana) serão descartadas.
Para evitar a perda de dados, você pode configurar um armazenamento persistente
usando [volumes do Docker](https://docs.docker.com/storage/volumes/) ou
[bind mounts](https://docs.docker.com/storage/bind-mounts/) para o seu
contêiner.

> **Nota:** Embora ambos os métodos sejam semelhantes, existe uma pequena
> diferença.
> Se você deseja que o armazenamento seja totalmente gerenciado pelo Docker e
> acessado apenas por meio de contêineres Docker e da CLI do Docker, deve optar
> pelo uso de volumes do Docker.
> No entanto, se precisar de controle total sobre o armazenamento e quiser
> permitir que outros processos, além do Docker, acessem ou modifiquem a camada
> de armazenamento, então os *bind mounts* são a escolha adequada para o seu
> ambiente.

#### Use volumes do Docker (recomendado)

Utilize volumes do Docker quando desejar que a Docker Engine gerencie o volume
de armazenamento.

Para usar volumes do Docker como armazenamento persistente, siga estes passos:

1. Crie um volume do Docker para ser utilizado pelo contêiner do Grafana,
   atribuindo a ele um nome descritivo (por exemplo, `grafana-storage`).
   Execute o seguinte comando:

   ```bash
   # crie um volume persistente para seus dados
   docker volume create grafana-storage

   # verifique se o volume foi criado corretamente
   # você deve ver uma saída em formato JSON
   docker volume inspect grafana-storage
   ```

1. Inicie o contêiner do Grafana executando o seguinte comando:

   ```bash
   # inicie o grafana
   docker run -d -p 3000:3000 --name=grafana \
     --volume grafana-storage:/var/lib/grafana \
     grafana/grafana-enterprise
   ```

#### Use bind mounts

Se você planeja usar diretórios do host para o banco de dados ou para a
configuração ao executar o Grafana no Docker, deve iniciar o contêiner com um
usuário que tenha permissão para acessar e gravar no diretório mapeado.

Para usar bind mounts, execute o seguinte comando:

```bash
# crie um diretório para seus dados
mkdir data

# inicie o Grafana com seu ID de usuário e utilizando o diretório de dados
docker run -d -p 3000:3000 --name=grafana \
  --user "$(id -u)" \
  --volume "$PWD/data:/var/lib/grafana" \
  grafana/grafana-enterprise
```

### Use variáveis de ambiente para configurar o Grafana

O Grafana permite especificar configurações personalizadas usando
[variáveis de ambiente](../../configure-grafana/#override-configuration-with-environment-variables).

```bash
# habilita os logs de depuração

docker run -d -p 3000:3000 --name=grafana \
  -e "GF_LOG_LEVEL=debug" \
  grafana/grafana-enterprise
```

## Instale plugins no contêiner Docker

Você pode instalar plugins no Grafana a partir da página oficial e da comunidade
de [plugins](/grafana/plugins) ou usando uma URL personalizada para instalar um
plugin privado.
Esses plugins permitem adicionar novos tipos de visualização, fontes de dados e
aplicações para ajudar você a visualizar melhor seus dados.

Atualmente, o Grafana oferece suporte a três tipos de plugins: painel, fonte de
dados e aplicação.
Para obter mais informações sobre o gerenciamento de plugins, consulte
[Gerenciamento de Plugins](../../../administration/plugin-management/).

Para instalar plugins no contêiner Docker, siga estes passos:

1. Passe os plugins que deseja instalar para o Docker usando a variável de
   ambiente `GF_PLUGINS_PREINSTALL`, fornecendo uma lista separada por vírgulas.

   Isso inicia um processo em segundo plano que instala a lista de plugins
   enquanto o servidor Grafana é iniciado.

   Por exemplo:

   ```bash
   docker run -d -p 3000:3000 --name=grafana \
     -e "GF_PLUGINS_PREINSTALL=grafana-clock-panel, yesoreyeram-infinity-datasource" \
     grafana/grafana-enterprise
   ```

1. Para especificar a versão de um plugin, adicione o número da versão à
   variável de ambiente `GF_PLUGINS_PREINSTALL`.

   Por exemplo:

   ```bash
   docker run -d -p 3000:3000 --name=grafana \
     -e "GF_PLUGINS_PREINSTALL=grafana-clock-panel@1.0.1" \
     grafana/grafana-enterprise
   ```

   > **Nota:** Se você não especificar um número de versão, a versão mais
   > recente será utilizada.

1. Para instalar um plugin a partir de uma URL personalizada, utilize a seguinte
   convenção para especificar a URL:
   `<ID do plugin>@[<versão do plugin>]@<URL do arquivo zip do plugin>`.

   Por exemplo:

   ```bash
   docker run -d -p 3000:3000 --name=grafana \
     -e "GF_PLUGINS_PREINSTALL=custom-plugin@@https://github.com/VolkovLabs/custom-plugin.zip" \
     grafana/grafana-enterprise
   ```

## Exemplo

O exemplo a seguir executa a versão estável mais recente do Grafana, escutando
na porta 3000, com o contêiner nomeado como `grafana`, armazenamento persistente
no volume Docker `grafana-storage`, a URL raiz do servidor definida e o plugin
oficial [clock panel](/grafana/plugins/grafana-clock-panel) instalado.

```bash
# crie um volume persistente para seus dados
docker volume create grafana-storage

# inicie o grafana utilizando o armazenamento persistente acima
# e definindo variáveis de ambiente

docker run -d -p 3000:3000 --name=grafana \
  --volume grafana-storage:/var/lib/grafana \
  -e "GF_SERVER_ROOT_URL=http://my.grafana.server/" \
  -e "GF_PLUGINS_PREINSTALL=grafana-clock-panel" \
  grafana/grafana-enterprise
```

## Execute o Grafana via Docker Compose

O Docker Compose é uma ferramenta de software que facilita a definição e o
compartilhamento de aplicações compostas por múltiplos contêineres.
Ele funciona utilizando um arquivo YAML, geralmente chamado de
`docker-compose.yaml`, que lista todos os serviços que compõem a aplicação.
É possível iniciar os contêineres na ordem correta com um único comando e, com
outro comando, encerrá-los.
Para mais informações sobre os benefícios e o uso do Docker Compose, consulte
[Use o Docker Compose](https://docs.docker.com/get-started/08_using_compose/).

### Antes de começar

Para executar o Grafana via Docker Compose, instale a ferramenta Compose em sua
máquina.
Para verificar se a ferramenta Compose está disponível, execute o seguinte
comando:

```bash
docker compose version
```

Se a ferramenta Compose não estiver disponível, consulte
[Instale o Docker Compose](https://docs.docker.com/compose/install/).

### Execute a versão estável mais recente do Grafana

Esta seção mostra como executar o Grafana usando o Docker Compose.
Os exemplos nesta seção utilizam a versão 3 do Compose.
Para mais informações sobre compatibilidade, consulte a
[matriz de compatibilidade entre Compose e Docker](https://docs.docker.com/compose/compose-file/compose-file-v3/).

> **Nota:** Se você estiver em um sistema Linux (por exemplo, Debian ou Ubuntu),
> pode ser necessário adicionar `sudo` antes do comando ou incluir seu usuário
> no grupo `docker`.
> Para mais informações, consulte os
> [Passos da pós-instalação da Docker Engine no Linux](https://docs.docker.com/engine/install/linux-postinstall/).

Para executar a versão estável mais recente do Grafana usando o Docker Compose,
siga estes passos:

1. Crie um arquivo `docker-compose.yaml`.

   ```bash
   # primeiro, entre no diretório onde você deseja criar este arquivo
   # docker-compose.yaml
   cd /path/to/docker-compose-directory

   # agora, crie o arquivo docker-compose.yaml
   touch docker-compose.yaml
   ```

1. Agora, adicione o seguinte código ao arquivo `docker-compose.yaml`.

   Por exemplo:

   ```yaml
   services:
     grafana:
       image: grafana/grafana-enterprise
       container_name: grafana
       restart: unless-stopped
       ports:
         - '3000:3000'
   ```

1. Para executar o `docker-compose.yaml`, execute o seguinte comando:

   ```bash
   # inicie o contêiner do Grafana
   docker compose up -d
   ```

   Onde:

   d = modo detached (em segundo plano)

   up = para iniciar e colocar o contêiner em execução

Para verificar se o Grafana está em execução, abra uma janela do navegador e
digite `IP_ADDRESS:3000`.
A tela de login deverá aparecer.

### Pare o contêiner do Grafana

Para parar o contêiner do Grafana, execute o seguinte comando:

```bash
docker compose down
```

> **Nota:** Para mais informações sobre o uso de comandos do Docker Compose,
> consulte
> [docker compose](https://docs.docker.com/engine/reference/commandline/compose/).

### Salve seus dados do Grafana

Por padrão, o Grafana utiliza um banco de dados SQLite versão 3 embutido para
armazenar configurações, usuários, dashboards e outros dados.
Ao executar imagens do Docker como contêineres, as alterações nesses dados do
Grafana são gravadas no sistema de arquivos dentro do contêiner, persistindo
apenas enquanto o contêiner existir.
Se você parar e remover o contêiner, quaisquer alterações no sistema de arquivos
(ou seja, os dados do Grafana) serão descartadas.
Para evitar a perda de dados, você pode configurar um armazenamento persistente
usando [volumes do Docker](https://docs.docker.com/storage/volumes/) ou
[bind mounts](https://docs.docker.com/storage/bind-mounts/) para o seu
contêiner.

#### Use volumes do Docker (recomendado)

Utilize volumes do Docker quando desejar que a Docker Engine gerencie o volume
de armazenamento.

Para usar volumes do Docker como armazenamento persistente, siga estes passos:

1. Crie um arquivo `docker-compose.yaml`

   ```bash
   # primeiro, entre no diretório onde você deseja criar este arquivo
   # docker-compose.yaml
   cd /path/to/docker-compose-directory

   # agora, crie o arquivo docker-compose.yaml
   touch docker-compose.yaml
   ```

1. Adicione o seguinte código ao arquivo `docker-compose.yaml`.

   ```yaml
   services:
     grafana:
       image: grafana/grafana-enterprise
       container_name: grafana
       restart: unless-stopped
       ports:
         - '3000:3000'
       volumes:
         - grafana-storage:/var/lib/grafana
   volumes:
     grafana-storage: {}
   ```

1. Salve o arquivo e execute o seguinte comando:

   ```bash
   docker compose up -d
   ```

#### Use bind mounts

Se você planeja usar diretórios do host para o banco de dados ou para a
configuração ao executar o Grafana no Docker, deve iniciar o contêiner com um
usuário que tenha permissão para acessar e gravar no diretório mapeado.

Para usar bind mounts, execute o seguinte comando:

1. Crie um arquivo `docker-compose.yaml`

   ```bash
   # primeiro, entre no diretório onde você deseja criar este arquivo
   # docker-compose.yaml
   cd /path/to/docker-compose-directory

   # agora, crie o arquivo docker-compose.yaml
   touch docker-compose.yaml
   ```

1. Crie o diretório onde você montará seus dados — neste caso, `/data` — por
   exemplo, no seu diretório de trabalho atual:

   ```bash
   mkdir $PWD/data
   ```

1. Agora, adicione o seguinte código ao arquivo `docker-compose.yaml`.

   ```yaml
   services:
     grafana:
       image: grafana/grafana-enterprise
       container_name: grafana
       restart: unless-stopped
       # se estiver executando como root, defina como 0
       # caso contrário, encontre o ID correto com o comando id -u
       user: '0'
       ports:
         - '3000:3000'
       # adicionando o ponto de montagem de volume que criamos anteriormente
       volumes:
         - '$PWD/data:/var/lib/grafana'
   ```

1. Salve o arquivo e execute o seguinte comando:

   ```bash
   docker compose up -d
   ```

### Exemplo

O exemplo a seguir executa a versão estável mais recente do Grafana, escutando
na porta 3000, com o contêiner nomeado como `grafana`, armazenamento persistente
no volume Docker `grafana-storage`, a URL raiz do servidor definida e o plugin
oficial [clock panel](/grafana/plugins/grafana-clock-panel) instalado.

```yaml
services:
  grafana:
    image: grafana/grafana-enterprise
    container_name: grafana
    restart: unless-stopped
    environment:
      - GF_SERVER_ROOT_URL=http://my.grafana.server/
      - GF_PLUGINS_PREINSTALL=grafana-clock-panel
    ports:
      - '3000:3000'
    volumes:
      - 'grafana_storage:/var/lib/grafana'
volumes:
  grafana_storage: {}
```

{{< admonition type="note" >}}
Se você quiser especificar a versão de um plugin, adicione o número da versão à
variável de ambiente `GF_PLUGINS_PREINSTALL`.
Por exemplo:
`-e "GF_PLUGINS_PREINSTALL=grafana-clock-panel@1.0.1,yesoreyeram-infinity-datasource@3.8.0"`.
Se você não especificar um número de versão, a versão mais recente será
utilizada.
{{< /admonition >}}

## Próximos passos

Consulte o guia de [Introdução](../../../getting-started/build-first-dashboard/)
para obter informações sobre como fazer login, configurar fontes de dados, entre
outros tópicos.

## Configure a imagem Docker

Consulte a página
[Configure uma imagem Docker do Grafana](../../configure-docker/) para obter
detalhes sobre as opções de personalização do ambiente, logs, banco de dados,
etc.

## Configure o Grafana

Consulte a página [Configuração](../../configure-grafana/) para obter detalhes sobre as opções de personalização do ambiente, logs, banco de dados, etc.
