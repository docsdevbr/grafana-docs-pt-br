---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/v13.2.3/docs/sources/setup-grafana/installation/_index.md
source_revision: 221de0dfb574b8148c3b4dd1d8d043dd109e3391
translation_status: ready

aliases:
  - ../install/
  - ../installation/
  - ../installation/installation/
  - ../installation/requirements/
  - /docs/grafana/v2.1/installation/install/
  - ./installation/rpm/
description: Guia de instalação do Grafana
labels:
  products:
    - enterprise
    - oss
title: Instale o Grafana
weight: 100
---

# Instale o Grafana

Esta página lista os requisitos mínimos de hardware e software para instalar o
Grafana.

Para executar o Grafana, você precisa de um sistema operacional suportado,
hardware que atenda ou supere os requisitos mínimos, um banco de dados
suportado e um navegador suportado.

O vídeo a seguir orienta você pelas etapas e comandos comuns para instalar o
Grafana em vários sistemas operacionais, conforme descrito neste documento.

{{< youtube id="f-x_p2lvz8s" >}}

O Grafana depende de outros softwares de código aberto para funcionar.
Para obter uma lista dos softwares de código aberto utilizados pelo Grafana,
consulte o arquivo
[package.json](https://github.com/grafana/grafana/blob/main/package.json).

## Sistemas operacionais suportados

O Grafana oferece suporte aos seguintes sistemas operacionais:

- [Debian ou Ubuntu](debian/)
- [RHEL ou Fedora](redhat-rhel-fedora/)
- [SUSE ou openSUSE](suse-opensuse/)
- [macOS](mac/)
- [Windows](windows/)

{{< admonition type="note" >}}
A instalação do Grafana em outros sistemas operacionais é possível, mas não é
recomendada nem suportada.
{{< /admonition >}}

## Recomendações de hardware

O Grafana requer os seguintes recursos mínimos de sistema:

- Memória mínima recomendada: 512 MB.
- CPU mínima recomendada: 1 núcleo.

Alguns recursos podem exigir mais memória ou CPUs.
Para mais informações, consulte as orientações de dimensionamento a seguir.

### Dimensionando a sua implantação

Estas diretrizes de dimensionamento abrangem apenas o processo do servidor
Grafana, ou seja, a interface da pessoa usuária (UI), o proxy de fonte de dados,
o mecanismo de alertas e o renderizador de imagens.
Elas não levam em conta os recursos necessários para as suas fontes de dados.
Armazenamentos de métricas como Prometheus ou Grafana Mimir, armazenamentos de
logs como Grafana Loki e backends de rastros como Grafana Tempo possuem, cada
um, seus próprios requisitos de hardware e capacidade.
Para obter orientações, consulte
[Planejamento de capacidade do Grafana Mimir](https://grafana.com/docs/mimir/latest/manage/run-production-environment/planning-capacity),
[Dimensionamento do cluster Loki](https://grafana.com/docs/loki/latest/setup/size)
e
[Planejamento da implantação do Tempo](https://grafana.com/docs/tempo/latest/set-up-for-tracing/setup-tempo/plan/).

Quatro fatores determinam mais diretamente as necessidades de recursos do
Grafana:

- **Pessoas usuárias simultâneas:** sessões de navegador ativas e simultâneas
  que realizam consultas ou provocam a atualização de painéis.
  Esse é o principal fator de carga para CPU e memória.
  Pessoas usuárias que mantêm o Grafana aberto, mas não estão visualizando
  ativamente os dashboards, geram pouca carga, a menos que esses dashboards
  tenham a atualização automática habilitada.
- **Regras de alerta:** carga de avaliação em segundo plano no agendador de
  alertas.
  Inúmeras regras com intervalos de avaliação curtos podem saturar a CPU,
  independentemente da atividade da pessoa usuária.
  No Grafana OSS, o mecanismo de alertas é executado no mesmo processo que a UI
  e o proxy de fonte de dados; portanto, a saturação da CPU causada pelos
  alertas compete diretamente com o desempenho das consultas dos dashboards.
  É por isso que isolar a avaliação de alertas em instâncias dedicadas é
  importante em implantações de grande escala.
  Consulte
  [Considerações sobre desempenho e limitações](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/alerting/set-up/performance-limitations/)
  para obter detalhes.
- **Fontes de dados:** embora o número de conexões de fontes de dados via proxy
  seja importante, o tipo de fonte é mais relevante.
  Plugins que utilizam o proxy de fonte de dados do backend do Grafana, a
  maioria das fontes SQL, como MySQL, PostgreSQL e Microsoft SQL Server, mantêm
  uma conexão aberta por consulta no servidor.
  Fontes de métricas baseadas em pull, como Prometheus ou Graphite, são
  consultadas de forma mais eficiente e geram menos carga para o processo do
  Grafana.
  Alguns plugins, como certas fontes de API pública ou o plugin Infinity,
  executam consultas diretamente no navegador e podem não gerar carga no
  servidor, dependendo da configuração do plugin e dos requisitos de
  autenticação.
  Uma implantação com cinco fontes de dados SQL acessadas via proxy e com alta
  demanda de consultas pode exigir mais recursos do que uma implantação com
  vinte fontes Prometheus.
- **Dashboards e painéis:** a quantidade de painéis e o intervalo de atualização
  determinam, em conjunto, o volume de consultas.
  Um dashboard com 30 painéis atualizados a cada 10 segundos gera,
  aproximadamente, seis vezes mais carga de consulta do que o mesmo dashboard
  atualizado a cada minuto.
  Dashboards com muitos painéis e intervalos de atualização curtos devem ser
  classificados em um nível superior ao que a simples contagem de dashboards
  sugeriria.
  Vale ressaltar que o Grafana Enterprise inclui cache de consultas, o que pode
  reduzir significativamente esse multiplicador quando muitas pessoas usuárias
  visualizam o mesmo dashboard simultaneamente, podendo até deslocar a
  implantação para um nível inferior.

A renderização de imagens e a existência de muitas regras de alerta com
intervalos curtos são as duas causas mais comuns de uma implantação superar o
dimensionamento inicial.
Configurações com múltiplas organizações e sincronização de diretórios via SSO
ou LDAP adicionam uma sobrecarga que pode elevar a implantação para o próximo
nível de exigência.

### Níveis de implantação

Use a tabela abaixo para identificar qual nível descreve sua carga de trabalho
e, em seguida, consulte a configuração de hardware de referência correspondente.

| Nível   | Pessoas usuárias simultâneas | Regras de alerta | Fontes de dados | Dashboards  |
|---------|------------------------------|------------------|-----------------|-------------|
| Pequeno | < 25                         | < 100            | < 5             | < 200       |
| Médio   | 25 – 200                     | 100 – 1,000      | 5 – 25          | 200 – 2,000 |
| Grande  | 200+                         | 1,000+           | 25+             | 2,000+      |

O limite de contagem de dashboard pressupõe, aproximadamente, de 10 a 20 painéis
individuais por dashboard geral, com intervalos de atualização de 30 segundos ou
mais.
Dashboards com mais painéis ou intervalos de atualização mais curtos geram uma
carga de consulta proporcionalmente maior e devem ser considerados como
pertencentes ao nível superior.
Da mesma forma, a contagem de fontes de dados pressupõe uma combinação de tipos
de fonte.
Implantações que dependem fortemente de fontes SQL via proxy devem planejar a
adoção do próximo nível superior.

Esses limites servem como ponto de partida.
Valide o dimensionamento com um teste de carga que reflita a complexidade real
dos seus dashboards, a quantidade de elementos e as taxas de atualização antes
de definir o hardware de produção.
Dimensione o ambiente para a carga de trabalho atual e reserve uma margem de
capacidade para picos de tráfego e crescimento.

#### Pequeno

Implantações de pequeno porte são adequadas para equipes pequenas, ferramentas
internas e ambientes de baixo tráfego.

| Recurso   | Mínimo                                  |
| --------- |-----------------------------------------|
| CPU       | 2 núcleos                               |
| Memória   | 2 – 4 GB                                |
| Disco     | 10 – 20 GB SSD (host do banco de dados) |
| Instâncias| 1                                       |

**Banco de dados:** o SQLite funciona para desenvolvimento local e pequenas
instâncias de avaliação, mas não é recomendado para ambientes de produção.
Para uso em produção, considere uma instância externa de MySQL ou PostgreSQL
para maior confiabilidade e capacidade de expansão.
Para mais informações, consulte
[Bancos de dados suportados](#bancos-de-dados-suportados).

**Renderização de imagens:** opcional; pode ser executada no mesmo host para uso
leve.
Consulte
[Configure a renderização de imagens](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/image-rendering/).

#### Médio

Implantações de porte médio são adequadas para ambientes de equipe
compartilhados e plataformas de observabilidade departamentais.

| Recurso   | Recomendação                            |
| --------- |-----------------------------------------|
| CPU       | 4 – 8 núcleos                           |
| Memória   | 8 – 16 GB                               |
| Disco     | 20 – 50 GB SSD (host do banco de dados) |
| Instâncias| 2 (com balanceamento de carga)          |

**Banco de dados:** o SQLite não é recomendado para ambientes de produção nem é
adequado para este nível de implantação.
Utilize um banco de dados externo MySQL ou PostgreSQL.
Consulte [Bancos de dados suportados](#bancos-de-dados-suportados) para obter
orientações sobre a escolha de um banco de dados externo.

**Renderização de imagens:** execute o renderizador de imagens como um processo
ou contêiner separado.
Cada worker de renderização consome aproximadamente 1 GB de memória; dimensione
o host do renderizador de acordo.
Consulte
[Configurar a renderização de imagens](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/image-rendering/).

**Alta disponibilidade:** se você executar duas ou mais instâncias do Grafana,
configure um armazenamento de sessões Redis ou habilite sessões persistentes no
balanceador de carga para evitar que as pessoas usuárias sejam desconectadas
entre as requisições.
Consulte
[Configure o Grafana para alta disponibilidade](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/set-up-for-high-availability/).

#### Grande

Implantações de grande porte são adequadas para plataformas que abrangem toda a
organização e para ambientes de produção com alto tráfego.

| Recurso   | Recomendação                                                                            |
| --------- |-----------------------------------------------------------------------------------------|
| CPU       | 8 – 16+ núcleos por instância                                                           |
| Memória   | 16 – 32+ GB por instância                                                               |
| Disco     | 50+ GB SSD, alto número de operações de E/S por segundo (IOPS) (host do banco de dados) |
| Instâncias| 3+ (com balanceamento de carga)                                                         |
| Rede      | 10 Gbps ou superior                                                                     |

**Banco de dados:** o SQLite não é recomendado para ambientes de produção nem é
adequado para este nível de implantação.
Recomenda-se fortemente o uso de um cluster MySQL ou PostgreSQL de alta
disponibilidade.
Consulte [Bancos de dados suportados](#bancos-de-dados-suportados).

**Renderização de imagens:** execute um conjunto dedicado de renderizadores com
múltiplos workers, isolados das instâncias do Grafana.
Cada worker de renderização utiliza aproximadamente 1 GB de memória.
Consulte
[Configure a renderização de imagens](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/image-rendering/).

**Avaliação de alertas:** com mais de 1.000 regras de alerta ou intervalos de
avaliação curtos inferiores a um minuto, a avaliação de alertas pode saturar a
CPU e prejudicar o desempenho das consultas de dashboards na mesma instância.
Para evitar isso, isole a avaliação de alertas em uma ou mais instâncias
dedicadas do Grafana operando em
[modo de avaliação remota](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/alerting/).
Consulte
[Considerações de desempenho e limitações](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/alerting/set-up/performance-limitations/).

**Alta disponibilidade:** são necessárias sessões persistentes ou um
armazenamento de sessão Redis compartilhado.
Consulte
[Configure o Grafana para alta disponibilidade](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/set-up-for-high-availability/).

**Latência da fonte de dados:** minimize os saltos de rede entre as instâncias
do Grafana e as fontes de dados.
Conexões de baixa latência com seu banco de dados e fontes de dados são
importantes nesta escala.

**Modelo de implantação:** gerenciar três ou mais instâncias do Grafana
juntamente com um cluster Redis, um conjunto de instâncias de renderização e um
banco de dados de alta disponibilidade torna-se operacionalmente complexo em
ambientes bare metal.
O Kubernetes reduz significativamente essa carga operacional nesse nível.
Consulte
[Implante o Grafana no Kubernetes](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/installation/kubernetes/)
e o
[chart Helm do Grafana](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/installation/helm/)
para obter orientações.

## Bancos de dados suportados

O Grafana requer um banco de dados para armazenar seus dados de configuração,
como usuários, fontes de dados e dashboards.
Os requisitos exatos dependem do tamanho da instalação do Grafana e dos recursos
utilizados.

O Grafana oferece suporte aos seguintes bancos de dados:

- [SQLite 3](https://www.sqlite.org/index.html)
- [MySQL 8.0+](https://www.mysql.com/support/supportedplatforms/database.html)
- [PostgreSQL 12+](https://www.postgresql.org/support/versioning/)

Por padrão, o Grafana utiliza um banco de dados SQLite embutido, armazenado no
local de instalação do Grafana.
Caso precise migrar para um banco de dados diferente posteriormente, observe que
as migrações de esquema e de dados do banco de dados são operações gerenciadas
pelo cliente e estão fora do escopo do Suporte do Grafana.

{{< admonition type="caution" >}}
O SQLite não é recomendado para ambientes de produção.
Ele funciona bem para desenvolvimento local e pequenas instâncias de avaliação,
mas não escala para cargas de trabalho de produção.
Se você deseja
[alta disponibilidade](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/set-up-for-high-availability/),
deve utilizar um banco de dados MySQL ou PostgreSQL.
Para obter informações sobre como definir os parâmetros de configuração do banco
de dados no arquivo `grafana.ini`, consulte
[[database]](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/setup-grafana/configure-grafana/#database).
{{< /admonition >}}

O Grafana oferece suporte às versões desses bancos de dados, oficialmente
suportadas pelo projeto no momento do lançamento de uma versão do Grafana.
Quando uma versão do Grafana deixa de ser suportada, a Grafana Labs também pode
encerrar o suporte para aquela versão do banco de dados.
Consulte os links acima para verificar as políticas de suporte de cada projeto.

{{< admonition type="note" >}}
As versões 10.9, 11.4 e 12-beta2 do PostgreSQL são afetadas por um erro
(rastreado pelo projeto PostgreSQL como
[erro #15865](https://www.postgresql.org/message-id/flat/15865-17940eacc8f8b081%40postgresql.org))
que impede o uso dessas versões com o Grafana.
O erro foi corrigido em versões mais recentes do PostgreSQL.
{{< /admonition >}}

{{< admonition type="note" >}}
Binários e imagens do Grafana podem não funcionar com bancos de dados não
suportados, mesmo que estes aleguem ser substitutos diretos ou repliquem a API
da melhor forma possível.
Binários e imagens compilados com
[BoringCrypto](https://pkg.go.dev/crypto/internal/boring) podem apresentar
problemas diferentes daqueles encontrados em outras distribuições do Grafana.
{{< /admonition >}}

> O Grafana pode apresentar erros ao utilizar servidores MySQL somente leitura,
> como em cenários de failover de alta disponibilidade ou no AWS Aurora MySQL
> serverless.
> Este é um problema conhecido; para mais informações, consulte a
> [issue #13399](https://github.com/grafana/grafana/issues/13399).

## Navegadores da web suportados

O Grafana oferece suporte à versão atual dos seguintes navegadores.
Versões mais antigas desses navegadores podem não ser suportadas; portanto, você
deve sempre atualizar para a versão mais recente do navegador ao usar o Grafana.

{{< admonition type="note" >}}
Ative o JavaScript no seu navegador.
Não há suporte para a execução do Grafana sem o JavaScript ativado no navegador.
{{< /admonition >}}

- Chrome/Chromium
- Firefox
- Safari
- Microsoft Edge

## Perguntas frequentes

{{< qa-list >}}
{{< qa question="Quais sistemas operacionais o Grafana suporta?" >}}
O Grafana suporta Debian e Ubuntu, RHEL e Fedora, SUSE e openSUSE, macOS e
Windows.
Você também pode executar o Grafana em um contêiner com o Docker ou implantá-lo
no Kubernetes usando o chart Helm do Grafana.
A instalação do Grafana em outros sistemas operacionais é possível, mas não é
recomendada nem suportada.
Para etapas específicas da plataforma, consulte o guia de instalação do seu
sistema operacional.
{{< /qa >}}
{{< qa
  question="Quais são as recomendações de hardware para executar o Grafana?" >}}
No mínimo, o Grafana requer 512 MB de memória e 1 núcleo de CPU, mas esse é um
requisito básico para avaliação, e não uma meta para produção.
Os requisitos reais dependem de quatro fatores: pessoas usuárias simultâneas,
número de regras de alerta e frequência de avaliação, número e tipo de fontes de
dados, e a quantidade de painéis e intervalos de atualização dos dashboards.
A documentação classifica as cargas de trabalho em níveis pequeno, médio e
grande, com uma configuração de hardware de referência para cada um.
Por exemplo, uma implantação pequena começa com 2 núcleos e 2 a 4 GB de memória,
enquanto uma implantação grande executa múltiplas instâncias com balanceamento
de carga, utilizando de 8 a 16+ núcleos e 16 a 32+ GB de memória cada.
Essas orientações abrangem apenas o processo do servidor Grafana.
Elas não levam em conta suas fontes de dados, pois backends de métricas, logs e
rastros, como Grafana Mimir, Grafana Loki e Grafana Tempo, possuem seus próprios
requisitos de capacidade.
Considere os níveis como pontos de partida e valide-os com um teste de carga que
reflita seus dashboards reais antes de investir em hardware de produção.
Para mais informações, consulte "Dimensionando a sua implantação".
{{< /qa >}}
{{< qa question="De quais recursos preciso para executar o Grafana?" >}}
Para executar o Grafana, você precisa de quatro coisas:

- Um sistema operacional suportado.
- Hardware que atenda ou supere os requisitos mínimos.
- Um banco de dados suportado.
- Um navegador suportado.

Além disso, planeje os componentes que escalam conforme a sua carga de trabalho:

- Um banco de dados MySQL ou PostgreSQL externo para qualquer cenário que vá
  além de uma pequena instância de avaliação.
- Um host ou contêiner separado para renderização de imagens.
  Cada worker de renderização utiliza aproximadamente 1 GB de memória.
- Sessões persistentes ou um armazenamento de sessão Redis compartilhado caso
  você execute mais de uma instância do Grafana para garantir alta
  disponibilidade.

Lembre-se de que o dimensionamento das suas fontes de dados é feito
separadamente do dimensionamento do próprio Grafana.
{{< /qa >}}
{{< qa question="Quais bancos de dados o Grafana suporta?" >}}
O Grafana armazena seus próprios dados de configuração, usuários, fontes de
dados, dashboards, etc., em um banco de dados e oferece suporte aos seguintes:

- SQLite 3.
- MySQL 8.0 ou superior.
- PostgreSQL 12 ou superior.

Novas instalações utilizam, por padrão, um banco de dados SQLite embutido,
armazenado no diretório de instalação do Grafana.
O SQLite funciona bem para desenvolvimento local e pequenas instâncias de
avaliação, mas não é recomendado para produção; além disso, a alta
disponibilidade exige MySQL ou PostgreSQL.
O Grafana oferece suporte às versões de banco de dados oficialmente suportadas
upstream no momento do lançamento de uma determinada versão do Grafana;
portanto, o suporte a uma versão de banco de dados pode ser descontinuado quando
a versão correspondente do Grafana deixar de receber suporte.
A migração entre bancos de dados é uma operação gerenciada pelo cliente e está
fora do escopo do Suporte do Grafana; por isso, escolha seu backend antes de
escalar a infraestrutura.
Isso é diferente das fontes de dados que o Grafana consulta para visualização,
as quais são abordadas na documentação de fontes de dados.
{{< /qa >}}
{{< /qa-list >}}
