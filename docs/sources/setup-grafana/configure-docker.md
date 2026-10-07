---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/v13.2.3/docs/sources/setup-grafana/configure-docker.md
source_revision: 116499590010626ff143e2290b1ea266cddabc84
translation_status: ready

aliases:
  - ../administration/configure-docker/
  - ../installation/configure-docker/
description: Guia para configurar a imagem Docker do Grafana.
keywords:
  - grafana
  - configuration
  - documentation
  - docker
  - docker compose
labels:
  products:
    - enterprise
    - oss
menuTitle: Configure uma imagem Docker
title: Configure uma imagem Docker do Grafana
weight: 1800
---

{{< admonition type="caution" >}}
A partir da versão `12.4.0` do Grafana, o repositório `grafana/grafana-oss` no
Docker Hub não será mais atualizado.
Em vez disso, recomendamos utilizar o repositório `grafana/grafana` no Docker
Hub.
Ambos os repositórios contêm as mesmas imagens Docker do Grafana OSS.
{{< /admonition >}}

# Configure uma imagem Docker do Grafana

Este tópico explica como executar o Grafana no Docker em ambientes complexos que
exigem:

- Usar imagens diferentes.
- Alterar níveis de log.
- Definir segredos na nuvem.
- Configurar plugins.

> **Nota:** Os exemplos neste tópico utilizam a imagem Docker do Grafana
> Enterprise.
> Você pode utilizar a edição Grafana Open Source alterando a imagem Docker para
> `grafana/grafana`.

## Variantes de imagens Docker suportadas

Você pode instalar e executar o Grafana utilizando as seguintes imagens Docker
oficiais.

- **Grafana Enterprise**: `grafana/grafana-enterprise`

- **Grafana Open Source**: `grafana/grafana`

Cada edição está disponível com uma imagem base Alpine, Ubuntu ou Distroless.
Cada imagem base também possui uma variante slim.

Adicione o sufixo da variante à versão do Grafana na tag da imagem:

| Imagem base | Tag padrão             | Tag slim                    |
|-------------| ---------------------- | --------------------------- |
| Alpine      | `<version>`            | `<version>-slim`            |
| Ubuntu      | `<version>-ubuntu`     | `<version>-ubuntu-slim`     |
| Distroless  | `<version>-distroless` | `<version>-distroless-slim` |

## Imagem Alpine (recomendada)

O [Alpine Linux](https://alpinelinux.org/about/) é uma distribuição Linux não
vinculada a nenhuma entidade comercial.
É um sistema operacional versátil que atende pessoas usuárias que priorizam
segurança, eficiência e facilidade de uso.
O Alpine Linux é muito menor do que outras imagens base de distribuição,
permitindo a criação de imagens mais leves e seguras.

Por padrão, as imagens são construídas utilizando a imagem base do
[projeto Alpine Linux](http://alpinelinux.org/), amplamente utilizado, que pode
ser encontrada no
[repositório Docker do Alpine](https://hub.docker.com/_/alpine).
Se você prioriza a segurança e deseja minimizar o tamanho da sua imagem,
recomenda-se utilizar a variante Alpine.
No entanto, é importante observar que a variante Alpine utiliza a
[musl libc](http://www.musl-libc.org/) em vez da
[glibc e outras](http://www.etalabs.net/compare_libcs.html).
Como resultado, alguns softwares podem apresentar problemas dependendo de seus
requisitos de libc.
Ainda assim, a maioria dos softwares não deve apresentar problemas, tornando a
variante Alpine geralmente confiável.

## Imagem Ubuntu

As imagens do Grafana Enterprise e OSS baseadas no Ubuntu são construídas
utilizando a imagem base do [Ubuntu](https://ubuntu.com/), que pode ser
encontrada no [repositório Docker do Ubuntu](https://hub.docker.com/_/ubuntu).
Uma imagem baseada no Ubuntu pode ser uma boa opção para pessoas usuárias que
preferem essa base ou que necessitam de determinadas ferramentas indisponíveis
no Alpine.

- **Grafana Enterprise**: `grafana/grafana-enterprise:<version>-ubuntu`

- **Grafana Open Source**: `grafana/grafana:<version>-ubuntu`

## Imagem Distroless

As imagens do Grafana Enterprise e OSS baseadas em Distroless utilizam a imagem
base [Distroless](https://github.com/GoogleContainerTools/distroless).
Imagens Distroless contêm menos pacotes do sistema operacional do que as imagens
Alpine e Ubuntu.
Elas não incluem shell, gerenciador de pacotes ou outros utilitários de sistema
operacional de uso geral, o que resulta em um tamanho de imagem menor.

- **Grafana Enterprise**: `grafana/grafana-enterprise:<version>-distroless`

- **Grafana Open Source**: `grafana/grafana:<version>-distroless`

## Imagens slim

As imagens slim não incluem os plugins que o Grafana disponibiliza nas imagens
padrão.
Você ainda pode instalar plugins na inicialização do contêiner definindo a
variável de ambiente `GF_PLUGINS_PREINSTALL`.
Para obter instruções, consulte
[Instalar plugins no contêiner Docker](../installation/docker/#install-plugins-in-the-docker-container).

Para usar uma imagem slim, adicione `-slim` ao sufixo da imagem base.
Por exemplo, use `<version>-slim` para Alpine, `<version>-ubuntu-slim` para
Ubuntu ou `<version>-distroless-slim` para Distroless.

## Execute uma versão específica do Grafana

Você também pode executar uma versão específica do Grafana ou uma versão beta
baseada na branch principal do repositório
[`grafana/grafana` no GitHub](https://github.com/grafana/grafana).

> **Nota:** Se você utiliza um sistema operacional Linux, como Debian ou Ubuntu,
> e encontrar erros de permissão ao executar comandos do Docker, pode ser
> necessário adicionar `sudo` antes do comando ou incluir seu usuário no grupo
> `docker`.
> A documentação oficial do Docker fornece instruções sobre como
> [executar o Docker com um usuário não root](https://docs.docker.com/engine/install/linux-postinstall/).

Para executar uma versão específica do Grafana, insira-a na seção
`<version number>` do comando:

```bash
docker run -d -p 3000:3000 --name grafana grafana/grafana-enterprise:<version number>
```

Exemplo:

O comando a seguir executa o contêiner do Grafana Enterprise e especifica a
versão 9.4.7.
Se você quiser executar uma versão diferente, modifique a seção do número da
versão.

```bash
docker run -d -p 3000:3000 --name grafana grafana/grafana-enterprise:9.4.7
```

Para lançamentos recentes do Grafana, também existem tags de versão `minor` nos
repositórios Docker `grafana/grafana` e `grafana/grafana-enterprise`.
Por exemplo, se você quiser ter sempre a versão `12.1` mais recente, pode usar
`grafana/grafana-enterprise:12.1` ou `grafana/grafana-enterprise:12.1-ubuntu`.

## Execute a branch principal do Grafana

Após cada construção bem-sucedida da branch principal, duas tags,
`grafana/grafana:main` e `grafana/grafana:main-ubuntu`, são atualizadas.
Além disso, duas novas tags são criadas: `grafana/grafana-dev:<version>` e
`grafana/grafana-dev:<version>-ubuntu`, onde `version` é uma versão de
pré-lançamento do Grafana.
Por exemplo, se `1234` for o ID da execução do GitHub para a construção, a
versão seria `12.2.0-1234`.
Essas tags fornecem acesso às construções mais recentes da branch principal do
Grafana.
Para mais informações, consulte
[`grafana/grafana-dev`](https://hub.docker.com/r/grafana/grafana-dev/tags).

Para garantir estabilidade e consistência, recomendamos fortemente o uso da tag
`grafana/grafana-dev:<version>` ao executar a branch principal do Grafana em um
ambiente de produção.
Essa tag assegura que você utilize uma versão específica do Grafana em vez do
commit mais recente, o qual poderia potencialmente introduzir erros ou
problemas.
Ela também evita poluir o namespace de tags das imagens principais do Grafana
com milhares de tags de pré-lançamento.

Para obter uma lista das tags disponíveis, consulte
[`grafana/grafana`](https://hub.docker.com/r/grafana/grafana/tags/) e
[`grafana/grafana-dev`](https://hub.docker.com/r/grafana/grafana-dev/tags/).

## Caminhos padrão

O Grafana vem com parâmetros de configuração padrão que permanecem inalterados
entre as versões, independentemente do sistema operacional ou do ambiente (por
exemplo, máquina virtual, Docker, Kubernetes, etc.).
Você pode consultar a documentação [Configure o Grafana](../configure-grafana/)
para ver todas as configurações padrão.

As seguintes configurações são definidas por padrão ao iniciar o contêiner
Docker do Grafana.
Ao executar no Docker, não é possível alterar as configurações editando o
arquivo `conf/grafana.ini`.
Em vez disso, você pode modificar a configuração usando
[variáveis de ambiente](../configure-grafana/#override-configuration-with-environment-variables).

| Configuração          | Valor padrão              |
| --------------------- | ------------------------- |
| GF_PATHS_CONFIG       | /etc/grafana/grafana.ini  |
| GF_PATHS_DATA         | /var/lib/grafana          |
| GF_PATHS_HOME         | /usr/share/grafana        |
| GF_PATHS_LOGS         | /var/log/grafana          |
| GF_PATHS_PLUGINS      | /var/lib/grafana/plugins  |
| GF_PATHS_PROVISIONING | /etc/grafana/provisioning |

## Instale plugins no contêiner Docker

Você pode instalar plugins disponíveis publicamente, bem como plugins privados
ou de uso interno em uma organização.
Para obter instruções sobre a instalação de plugins, consulte
[Instale plugins no contêiner Docker](../installation/docker/#install-plugins-in-the-docker-container).

### Instale plugins de outras fontes

Para instalar plugins de outras fontes, você deve definir a URL personalizada e
especificá-la imediatamente antes do nome do plugin na variável de ambiente
`GF_PLUGINS_PREINSTALL`: `GF_PLUGINS_PREINSTALL=<ID do plugin>@[<versão do plugin>]@<URL para o zip do plugin>`.

Exemplo:

O comando a seguir executa o Grafana Enterprise na **porta 3000** em segundo
plano e instala o plugin personalizado, especificado como um parâmetro de URL na
variável de ambiente `GF_PLUGINS_PREINSTALL`.

```bash
docker run -d -p 3000:3000 --name=grafana \
  -e "GF_PLUGINS_PREINSTALL=custom-plugin@@http://plugin-domain.com/my-custom-plugin.zip,grafana-clock-panel" \
  grafana/grafana-enterprise
```

## Crie uma imagem Docker personalizada do Grafana

No repositório do Grafana no GitHub, a pasta `packaging/docker/custom/` contém
um `Dockerfile` que você pode usar para criar uma imagem personalizada do
Grafana.
O `Dockerfile` aceita `GRAFANA_VERSION` e `GF_INSTALL_PLUGINS` como argumentos
de construção.

O argumento de construção `GRAFANA_VERSION` deve corresponder a uma tag válida
da imagem Docker `grafana/grafana`.
Por padrão, o Grafana cria uma imagem baseada em Alpine.
Para criar uma imagem baseada em Ubuntu, adicione `-ubuntu` ao argumento de
construção `GRAFANA_VERSION`.

Exemplo:

O exemplo a seguir mostra como criar e executar uma imagem Docker personalizada
do Grafana, baseada na imagem Docker oficial mais recente do Grafana com Ubuntu:

```bash
# acesse o diretório personalizado
cd packaging/docker/custom

# execute o comando docker build para construir a imagem
docker build \
  --build-arg "GRAFANA_VERSION=latest-ubuntu" \
  -t grafana-custom .

# execute o contêiner personalizado do Grafana usando o comando docker run
docker run -d -p 3000:3000 --name=grafana grafana-custom
```

### Crie uma imagem Docker do Grafana com plugins pré-instalados

Se você mantém várias instâncias do Grafana com os mesmos plugins, pode
economizar tempo criando uma imagem personalizada que inclua os plugins
disponíveis na
[página de download de plugins do Grafana](/grafana/plugins).
Ao criar uma imagem personalizada, o Grafana não precisa instalar os plugins a
cada inicialização, tornando o processo de inicialização mais eficiente.

> **Nota:** Para especificar a versão de um plugin, você pode usar o argumento
> de construção `GF_INSTALL_PLUGINS` e adicionar o número da versão.
> A versão mais recente é utilizada caso você não especifique um número de
> versão.
> Por exemplo, você pode usar
> `--build-arg "GF_INSTALL_PLUGINS=grafana-clock-panel 1.0.1,yesoreyeram-infinity-datasource 3.8.0"`
> para especificar as versões de dois plugins.

Exemplo:

O exemplo a seguir mostra como criar e executar uma imagem Docker personalizada
do Grafana com plugins pré-instalados.

```bash
# acesse o diretório custom
cd packaging/docker/custom

# execute o comando de build
# inclua os plugins desejados, por exemplo: clock panel, etc.
docker build \
  --build-arg "GRAFANA_VERSION=latest" \
  --build-arg "GF_INSTALL_PLUGINS=grafana-clock-panel,yesoreyeram-infinity-datasource" \
  -t grafana-custom .

# execute o contêiner do Grafana personalizado usando o comando docker run
docker run -d -p 3000:3000 --name=grafana grafana-custom
```

### Crie uma imagem Docker do Grafana com plugins pré-instalados de outras fontes

Você pode criar uma imagem Docker contendo um plugin exclusivo da sua
organização, mesmo que ele não esteja acessível ao público.
Basta usar o argumento de construção `GF_INSTALL_PLUGINS` para especificar a URL
do plugin e o nome da pasta de instalação, como em
`GF_INSTALL_PLUGINS=<url do zip do plugin>;<nome da pasta de instalação do plugin>`.

O exemplo a seguir demonstra a criação de uma imagem Docker personalizada do
Grafana que inclui um plugin customizado a partir de uma URL, o plugin clock
panel e o plugin simple-json-datasource.
Você pode definir esses plugins no argumento de construção utilizando a variável
de ambiente de plugins do Grafana.

```bash
# acesse a pasta
cd packaging/docker/custom

# execute o comando docker build
docker build \
  --build-arg "GRAFANA_VERSION=latest" \
  --build-arg "GF_INSTALL_PLUGINS=http://plugin-domain.com/my-custom-plugin.zip;my-custom-plugin,grafana-clock-panel,yesoreyeram-infinity-datasource" \
  -t grafana-custom .

# execute o comando docker run
docker run -d -p 3000:3000 --name=grafana grafana-custom
```

## Registro de logs

Por padrão, os logs dos contêineres Docker são direcionados para `STDOUT`, uma
prática comum na comunidade Docker.
Você pode alterar isso definindo um [modo de log](../configure-grafana/#mode)
diferente, como `console`, `file` ou `syslog`.
É possível utilizar um ou mais modos separando-os por espaços; por exemplo:
`console file`.
Por padrão, os modos `console` e `file` estão habilitados.

Exemplo:

O exemplo a seguir executa o Grafana utilizando o modo de log `console file`,
definido na variável de ambiente `GF_LOG_MODE`.

```bash
# Executa o Grafana registrando logs tanto na saída padrão (stdout) quanto em
# /var/log/grafana/grafana.log

docker run -p 3000:3000 -e "GF_LOG_MODE=console file" grafana/grafana-enterprise
```

## Configure o Grafana com Docker Secrets

Você pode inserir dados confidenciais, como credenciais de login e segredos, no
Grafana utilizando arquivos de configuração.
Esse método funciona bem com o
[Docker Secrets](https://docs.docker.com/engine/swarm/secrets/), pois os
segredos são mapeados automaticamente para o local `/run/secrets/` dentro do
contêiner.

Você pode aplicar essa técnica a qualquer opção de configuração do arquivo
`conf/grafana.ini` definindo `GF_<NomeDaSeção>_<NomeDaChave>__FILE` com o
caminho do arquivo que contém a informação secreta.
Para mais informações sobre o uso de comandos do Docker Secrets, consulte
[docker secret](https://docs.docker.com/engine/reference/commandline/secret/).

O exemplo a seguir demonstra como definir a senha de admin:

- Segredo da senha de admin: `/run/secrets/admin_password`
- Variável de ambiente: `GF_SECURITY_ADMIN_PASSWORD__FILE=/run/secrets/admin_password`

### Configure credenciais via Docker Secrets para o AWS CloudWatch

O Grafana inclui suporte nativo para a
[fonte de dados do Amazon CloudWatch](../../datasources/aws-cloudwatch/).
Para configurar a fonte de dados, é necessário fornecer informações como o ID da
chave da AWS, a chave de acesso secreta, a região, entre outras.
Você pode utilizar o Docker Secrets para fornecer essas informações.

Exemplo:

O exemplo abaixo demonstra como utilizar variáveis de ambiente do Grafana via
Docker Secrets para o ID da chave da AWS, a chave de acesso secreta, a região e
o perfil.

O exemplo utiliza os seguintes valores para a fonte de dados do AWS CloudWatch:

```bash
AWS_default_ACCESS_KEY_ID=aws01us02
AWS_default_SECRET_ACCESS_KEY=topsecret9b78c6
AWS_default_REGION=us-east-1
```

1. Crie um segredo do Docker para cada um dos valores anotados acima.

   ```bash
   echo "aws01us02" | docker secret create aws_access_key_id -
   ```

   ```bash
   echo "topsecret9b78c6" | docker secret create aws_secret_access_key -
   ```

   ```bash
   echo "us-east-1" | docker secret create aws_region -
   ```

1. Execute o seguinte comando para verificar se os segredos foram criados.

   ```bash
   $ docker secret ls
   ```

   A saída do comando deve ser semelhante ao seguinte:

   ```
   ID                          NAME           DRIVER    CREATED              UPDATED
   i4g62kyuy80lnti5d05oqzgwh   aws_access_key_id             5 minutes ago        5 minutes ago
   uegit5plcwodp57fxbqbnke7h   aws_secret_access_key         3 minutes ago        3 minutes ago
   fxbqbnke7hplcwodp57fuegit   aws_region                    About a minute ago   About a minute ago
   ```

   Onde:

   ID = o ID exclusivo do segredo que será usado no comando `docker run`.

   NAME = o nome lógico definido para cada segredo.

1. Adicione os secrets à linha de comando ao executar o Docker.

   ```bash
   docker run -d -p 3000:3000 --name grafana \
     -e "GF_DEFAULT_INSTANCE_NAME=my-grafana" \
     -e "GF_AWS_PROFILES=default" \
     -e "GF_AWS_default_ACCESS_KEY_ID__FILE=/run/secrets/aws_access_key_id" \
     -e "GF_AWS_default_SECRET_ACCESS_KEY__FILE=/run/secrets/aws_secret_access_key" \
     -e "GF_AWS_default_REGION__FILE=/run/secrets/aws_region" \
     -v grafana-data:/var/lib/grafana \
     grafana/grafana-enterprise
   ```

Você também pode especificar múltiplos perfis para `GF_AWS_PROFILES` (por
exemplo, `GF_AWS_PROFILES=default another`).

A lista a seguir inclui as variáveis de ambiente suportadas:

- `GF_AWS_${profile}_ACCESS_KEY_ID`: ID da chave de acesso da AWS (obrigatório).
- `GF_AWS_${profile}_SECRET_ACCESS_KEY`: Chave de acesso secreta da AWS
  (obrigatório).
- `GF_AWS_${profile}_REGION`: Região da AWS (opcional).

## Solução de problemas em uma implantação Docker

Por padrão, o nível de log do Grafana é definido como `INFO`, mas você pode
alterar o nível de log para o modo `DEBUG` quando quiser reproduzir um problema.

Para mais informações sobre logs, consulte [logs](../configure-grafana/#log).

### Aumente o nível de log usando o comando `docker run` (CLI)

Para alterar o nível de log para o modo `DEBUG`, adicione a variável de ambiente
`GF_LOG_LEVEL` à linha de comando.

```bash
docker run -d -p 3000:3000 --name=grafana \
  -e "GF_LOG_LEVEL=debug" \
  grafana/grafana-enterprise
```

### Aumente o nível de log usando o Docker Compose

Para alterar o nível de log para o modo `DEBUG`, adicione a variável de ambiente
`GF_LOG_LEVEL` ao arquivo `docker-compose.yaml`.

```yaml
version: '3.8'
services:
  grafana:
    image: grafana/grafana-enterprise
    container_name: grafana
    restart: unless-stopped
    environment:
      # aumenta o nível de log de info para debug
      - GF_LOG_LEVEL=debug
    ports:
      - '3000:3000'
    volumes:
      - 'grafana_storage:/var/lib/grafana'
volumes:
  grafana_storage: {}
```

### Valide o arquivo YAML do Docker Compose

A probabilidade de ocorrerem erros de sintaxe em um arquivo YAML aumenta à
medida que o arquivo se torna mais complexo.
Você pode usar o comando a seguir para verificar se há erros de sintaxe.

```bash
# acesse o diretório do seu docker-compose.yaml
cd /path-to/docker-compose/file

# execute o comando de validação
docker compose config
```

Se houver erros no arquivo YAML, a saída do comando destacará as linhas que
contêm os erros.
Se não houver erros no arquivo YAML, a saída exibirá o conteúdo do arquivo
`docker-compose.yaml` em formato YAML detalhado.
