---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/getting-started/first-dashboards/get-started-grafana-prometheus.md
revision: 83e190a160cc4ea88e606b9d43b09d4ab8ceb1fa
status: ready

aliases:
  - ../../../getting-started/getting-started-prometheus/ #/docs/grafana/latest/getting-started/getting-started-prometheus
  - ../../../getting-started/get-started-grafana-prometheus/
description: Aprenda a criar seu primeiro dashboard do Prometheus no Grafana.
labels:
  products:
    - enterprise
    - oss
title: Comece a usar o Grafana e o Prometheus
weight: 300
---

# Comece a usar o Grafana e o Prometheus

O Prometheus é um sistema de monitoramento de código aberto para o qual o
Grafana oferece suporte imediato.
Este tópico orienta você nos passos para criar uma série de dashboards no
Grafana para exibir métricas de sistema de um servidor monitorado pelo
Prometheus.

{{< admonition type="tip" >}}
Confira nossas **Jornadas de aprendizagem** do Prometheus.

- [Conecte-se a uma fonte de dados Prometheus no Grafana Cloud](https://www.grafana.com/docs/learning-journeys/prometheus/)
  para visualizar suas métricas diretamente de onde elas estão armazenadas.
- [Envie métricas para o Grafana Cloud usando a gravação remota do Prometheus](https://www.grafana.com/docs/learning-journeys/prom-remote-write/)
  para explorar o Grafana Cloud sem fazer alterações significativas em sua
  configuração existente.
{{< /admonition >}}

_Grafana e Prometheus_:

1. Baixe o Prometheus e o Node Exporter.
1. Instale o Prometheus Node Exporter.
1. Instale e configure o Prometheus.
1. Configure o Prometheus para o Grafana.
1. Verifique as métricas do Prometheus na visualização Explore do Grafana.
1. Comece a criar dashboards.

## Baixe o Prometheus e o Node Exporter

Baixe os seguintes componentes:

- [Prometheus](https://prometheus.io/download/#prometheus)
- [Node exporter](https://prometheus.io/download/#node_exporter)

Assim como o Grafana, você pode instalar o Prometheus em diversos sistemas
operacionais.
Consulte a [página de download do Prometheus](https://prometheus.io/download/)
para ver uma lista das versões estáveis dos componentes do Prometheus.

## Instale o Prometheus Node Exporter

Instale o Node Exporter em todos os hosts que você deseja monitorar.
Este guia mostra como instalá-lo localmente.

O Prometheus Node Exporter é uma ferramenta amplamente utilizada que expõe
métricas do sistema.
Para obter instruções sobre como instalar o Node exporter, consulte a seção
[Instalando e executando o Node exporter](https://prometheus.io/docs/guides/node-exporter/#installing-and-running-the-node-exporter)
na documentação do Prometheus.

Ao executar o Node exporter localmente, acesse `http://localhost:9100/metrics`
para verificar se ele está exportando métricas.

{{< admonition type="note" >}}
As instruções no tópico referenciado são destinadas a pessoas usuárias do Linux.
Você pode precisar adaptá-las ligeiramente dependendo do seu sistema
operacional.
Por exemplo, se você estiver no Windows, use o
[windows_exporter](https://github.com/prometheus-community/windows_exporter).
{{< /admonition >}}

## Instale e configure o Prometheus

1. Após [baixar o Prometheus](https://prometheus.io/download/#prometheus),
   extraia-o e navegue até o diretório.

   ```
   tar xvfz prometheus-*.tar.gz
   cd prometheus-*
   ```

1. Localize o arquivo `prometheus.yml` no diretório.

1. Modifique o arquivo de configuração do Prometheus para monitorar os hosts
   onde você instalou o Node Exporter.

Por padrão, o Prometheus procura o arquivo `prometheus.yml` no diretório de
trabalho atual.
Esse comportamento pode ser alterado através da flag de linha de comando
`--config.file`.
Por exemplo, alguns instaladores do Prometheus a utilizam para definir o arquivo
de configuração como `/etc/prometheus/prometheus.yml`.

O exemplo a seguir mostra o código que você deve adicionar.
Observe que os alvos de configuração estática são definidos como
`['localhost:9100']` para direcionar o node-explorer quando executado
localmente.

```
 # Uma configuração de coleta contendo exatamente um endpoint para coletar dados
 # do Node exporter em execução em um host:
 scrape_configs:
     # O nome da tarefa é adicionado como um rótulo `job=<nome_da_tarefa>` a
     # qualquer série temporal extraída dessa configuração.
     - job_name: 'node'

     # O padrão para metrics_path é '/metrics'
     # O padrão para scheme é 'http'.

       static_configs:
       - targets: ['localhost:9100']
```

1. Inicie o serviço Prometheus:

   ```
    ./prometheus --config.file=./prometheus.yml
   ```

1. Confirme se o Prometheus está em execução acessando `http://localhost:9090`.

Você pode ver que as métricas do Node Exporter foram entregues ao Prometheus.
Em seguida, as métricas serão enviadas ao Grafana.

## Configure o Prometheus para o Grafana

Ao executar o Prometheus localmente, existem duas maneiras de configurá-lo para
o Grafana.
Você pode usar uma instância hospedada do Grafana no [Grafana Cloud](/) ou
executar o Grafana localmente.

Este guia descreve a configuração do Prometheus em uma instância hospedada do
Grafana no Grafana Cloud.

1. Cadastre-se em [https://grafana.com/](/auth/sign-up/create-user).
   O Grafana fornece uma instância do Prometheus pronta para uso.

![Detalhes do Prometheus em Grafana.com](/static/img/docs/getting-started/screenshot-grafana-prometheus-details.png)

1. Como você está executando sua própria instância do Prometheus localmente,
   você precisa usar o comando `remote_write` para enviar suas métricas para a
   instância do Prometheus hospedada no Grafana.com.
   O Grafana fornece o código a ser adicionado ao seu arquivo de configuração
   `prometheus.yml`.
   Isso inclui um endpoint de escrita remota, seu nome de usuário e senha.

Adicione o seguinte código ao seu arquivo `prometheus.yml` para começar a enviar
métricas para sua instância hospedada do Grafana.

```
remote_write:
- url: <https://seu-endpoint-de-gravação-remota>
  basic_auth:
    username: <seu nome de usuário>
    password: <sua chave de API do Grafana.com>
```

{{< admonition type="note" >}}
Para configurar sua instância do Prometheus para funcionar com o Grafana
localmente em vez do Grafana Cloud, instale o Grafana [aqui](/grafana/download)
e siga as etapas de configuração listadas
[aqui](/docs/grafana/latest/datasources/prometheus/#configure-the-data-source).
{{< /admonition >}}

## Solução de problemas

Estas são algumas etapas de solução de problemas que você pode tentar se o
Prometheus não estiver em execução ou funcionando como esperado.
As etapas fornecidas foram selecionadas com base nas Jornadas de aprendizagem
que oferecemos para o Prometheus.
Se você quiser explorar mais, confira a
[Jornada de aprendizagem do Prometheus](https://grafana.com/docs/learning-journeys/prometheus/)
se desejar visualizar dados no Grafana Cloud sem enviar ou armazenar dados no
Grafana Cloud, como para necessidades de retenção local.
Como alternativa, se você já possui uma configuração do Prometheus e deseja
explorar o Grafana Cloud sem fazer alterações significativas, visite a
[Jornada de aprendizagem de gravação remota do Prometheus](https://grafana.com/docs/learning-journeys/prom-remote-write/).

### 1. Verificando se o Prometheus está em execução

Se a interface web do Prometheus estiver inacessível (por exemplo, erro
"Connection refused" no navegador) ou se as consultas ao Prometheus falharem
(por exemplo, erros no Grafana como "Data source unavailable" ou "No data
points"), um bom ponto de partida é confirmar se o processo e o serviço do
Prometheus estão em execução.

Você pode fazer isso verificando o processo do sistema ou o status do serviço:

**Linux**

```bash
sudo systemctl status prometheus
```

- Mostra se o processo está em execução e se o serviço está ativo.

**MacOS**

```bash
pgrep prometheus
```

- Retorna o ID do processo (PID) se o Prometheus estiver em execução.

**Windows** (`PowerShell`)

```powershell
Get-Process -Name prometheus -ErrorAction SilentlyContinue
```

- Verifica se o processo do Prometheus está em execução, pelo nome.

### 2. Se o Prometheus não estiver em execução

Comece verificando as causas comuns:

**Verifique se há conflitos de porta**.

O Prometheus utiliza a porta 9090 por padrão.
Se outro processo estiver usando essa porta, o Prometheus poderá falhar em
iniciar.
Você pode verificar se há conflitos de porta com:

**Linux e macOS**

```bash
lsof -i :9090
```

**Windows** (`PowerShell`)

```powershell
netstat -ano | findstr :9090
```

- Mostra se outro processo está usando a porta **9090**.

**Verifique a localização dos binários do Prometheus**: certifique-se de que os
binários do Prometheus (`prometheus` e `promtool`) estejam instalados
corretamente.

**Linux e macOS**

```bash
ls /usr/local/bin/prometheus /usr/local/bin/promtool
```

- Se estiverem ausentes, mova-os para `/usr/local/bin` ou para um diretório no
  **PATH** do seu sistema.

**Verifique se o Prometheus está no PATH**.

```bash
which prometheus
which promtool
```

- Se **não houver saída**, os binários não estão no **PATH** do sistema.

**Verifique se os arquivos de configuração e dados estão no lugar certo**.

**Linux e macOS**

```bash
ls /etc/prometheus /var/lib/prometheus
ls /etc/prometheus/prometheus.yml
```

- Certifica que o Prometheus tem os diretórios de configuração e armazenamento
  de dados necessários.

**Verifique as permissões**: Se o Prometheus estiver sendo executado como um
usuário dedicado, verifique se a propriedade está correta:

```bash
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
```

(Opcional) **Proteja o Prometheus criando um usuário dedicado**

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

- Recomendado por motivos de segurança: execute o Prometheus como um usuário sem
  privilégios de login.

### 3. Verificando se o Prometheus está sendo executado como um serviço

Se o Prometheus estiver sendo executado como um processo, verifique se ele está
configurado e gerenciado corretamente como um serviço para garantir que reinicie
automaticamente após reinicializações ou falhas.

**Verifique o status do serviço Prometheus**

**Linux**

```bash
systemctl status prometheus.service
```

**Windows**

```powershell
sc query prometheus
```

**macOS**

```bash
pgrep prometheus
```

- Se o serviço estiver **inativo (morto) ou parado**, prossiga para os próximos
  passos.

### 4. Se o Prometheus não estiver sendo executado como um serviço

Se o Prometheus não estiver sendo executado como um serviço gerenciado,
verifique se ele está configurado corretamente e se pode reiniciar
automaticamente.

**Verifique a configuração do serviço** **(Linux e macOS)**

Verifique o arquivo de unidade do serviço para garantir que os caminhos estejam
corretos:

```bash
sudo nano /etc/systemd/system/prometheus.service
```

- Procure pela linha `ExecStart`:

```bash
ExecStart=/usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus/
```

- Certifique-se de que:
  - O **caminho do binário** (`/usr/local/bin/prometheus`) esteja correto.
  - O **arquivo de configuração** (`/etc/prometheus/prometheus.yml`) esteja
    presente.
  - O **caminho de armazenamento** (`/var/lib/prometheus/`) exista.

**Reinicie e habilite o serviço Prometheus (Linux e macOS)**

```bash
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus
```

**Verifique o estado de saúde do Prometheus**

Após reiniciar, verifique se o Prometheus está respondendo:

```bash
curl -s http://localhost:9090/-/ready
```

- Se a operação for bem-sucedida, isso confirma que o Prometheus está **pronto
  para atender solicitações**.

**Reinicie o serviço Prometheus (Windows)**

Se estiver sendo executado como um serviço do Windows, reinicie-o:

```powershell
net stop prometheus
net start prometheus
```

### 5. Verificando se o Prometheus está capturando métricas

Se você instalou o [Node exporter](#install-prometheus-node-exporter) para expor
as métricas do seu sistema, você pode verificar se o Prometheus está capturando
métricas enviando uma requisição para o endpoint `/metrics`.

```bash
curl http://localhost:9090/metrics
```

- Deve retornar uma série de métricas e metadados sobre as métricas expostas.

## Verifique as métricas do Prometheus no Grafana Metrics Drilldown

Na sua instância do Grafana, acesse a visualização
[Drilldown](https://www.grafana.com/docs/grafana/latest/explore/simplified-exploration/metrics/)
e experimente a navegação sem consultas pelas métricas compatíveis com o
Prometheus.

## Começar a criar dashboards

Agora que você tem uma lista selecionada de consultas, crie
[dashboards](../../dashboards/) para exibir as métricas do sistema monitoradas
pelo Prometheus.
Ao instalar o Prometheus e o Node Exporter ou o windows_exporter, você
encontrará dashboards recomendados para uso.

A imagem a seguir mostra um dashboard com três painéis exibindo algumas métricas
do sistema.

![Dashboards do Prometheus](/static/img/docs/getting-started/simple_grafana_prom_dashboard.png)

Para saber mais:

- Documentação do Grafana: [Fonte de dados do Prometheus](../../datasources/prometheus/)
- Documentação do Prometheus: [O que é o Prometheus?](https://prometheus.io/docs/introduction/overview/)
