---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

source_url: https://github.com/grafana/grafana/blob/main/docs/sources/fundamentals/timeseries/index.md
revision: 8c7090bc110be5b651d02894a050dd33f331a021
status: ready

aliases:
  - ../basics/timeseries/
  - /docs/grafana-cloud/introduction/timeseries/
description: Introdução às séries temporais.
keywords:
  - grafana
  - intro
  - guide
  - concepts
  - timeseries
labels:
  products:
    - cloud
    - enterprise
    - oss
title: Introdução às séries temporais
weight: 400
---

# Introdução às séries temporais

Imagine que você queira saber como a temperatura externa varia ao longo do dia.
A cada hora, você verificaria o termômetro e anotaria o horário e a temperatura
atual.
Após um tempo, você teria algo assim:

| Tempo | Valor |
| ----- | ----- |
| 09:00 | 24°C  |
| 10:00 | 26°C  |
| 11:00 | 27°C  |

Dados de temperatura como estes são um exemplo do que chamamos de _série
temporal_ — uma sequência de medições, ordenadas no tempo.
Cada linha da tabela representa uma medição individual em um momento específico.

Tabelas são úteis quando se deseja identificar medições individuais, mas
dificultam a visualização do panorama.
Uma visualização mais comum para séries temporais é o _gráfico_, que, em vez
disso, posiciona cada medição ao longo de um eixo temporal.
Representações visuais como o gráfico facilitam a descoberta de padrões e
características dos dados que, de outra forma, seriam difíceis de perceber.

{{< figure src="/static/img/docs/example_graph.png" class="docs-image--no-shadow" max-width="850px" alt="Dados de temperatura exibidos em um dashboard" >}}

Dados de temperatura como os do exemplo estão longe de ser o único exemplo de
série temporal.
Outros exemplos de séries temporais são:

- Uso de CPU e memória
- Dados de sensores
- Índice do mercado de ações

Embora cada um desses exemplos seja uma sequência de medições ordenadas
cronologicamente, eles também compartilham outros atributos:

- Novos dados são adicionados ao final, em intervalos regulares — por exemplo, a
  cada hora, às 9h, 10h, 11h e assim por diante.
- As medições são raramente atualizadas após serem adicionadas — por exemplo, a
  temperatura de ontem não muda.

Séries temporais são poderosas.
Elas ajudam a entender o passado, permitindo analisar o estado do sistema em
qualquer ponto no tempo.
Uma série temporal pode indicar que o servidor travou momentos depois que o
espaço livre em disco chegou a zero.

Séries temporais também podem ajudar a prever o futuro, revelando tendências nos
dados.
Se o número de pessoas usuárias registradas tem aumentado 4% ao mês nos últimos
meses, você pode prever o tamanho da sua base de pessoas usuárias no final do
ano.

Algumas séries temporais apresentam padrões que se repetem ao longo de um
período conhecido.
Por exemplo, a temperatura geralmente é mais alta durante o dia e cai à noite.
Ao identificar essas séries temporais periódicas, ou _sazonais_, você pode fazer
previsões confiáveis sobre o próximo período.
Se você sabe que a carga do sistema atinge o pico todos os dias por volta das
18h, pode adicionar mais máquinas pouco antes desse horário.

## Agregando séries temporais

Dependendo do que você está medindo, os dados podem variar bastante.
E se você quisesse comparar períodos maiores do que o intervalo entre as
medições?
Se você medisse a temperatura uma vez por hora, obteria 24 pontos de dados por
dia.
Para comparar a temperatura em agosto ao longo dos anos, você teria que combinar
os 31 vezes 24 pontos de dados em um só.

Combinar uma coleção de medições é chamado de _agregação_.
Existem várias maneiras de agregar dados de séries temporais.
Aqui estão algumas das mais comuns:

- **Average** (média) retorna a soma de todos os valores dividida pelo número
  total de valores.
- **Min** (mínimo) e **Max** (máximo) retornam o menor e o maior valor da
  coleção.
- **Sum** (soma) retorna a soma de todos os valores da coleção.
- **Count** (contagem) retorna o número de valores na coleção.

Por exemplo, agregando os dados de um mês, você pode determinar que agosto de
2017 foi, em média, mais quente do que o ano anterior.
Em vez disso, para ver qual mês teve a temperatura mais alta, você compararia a
temperatura máxima de cada mês.

Como você escolhe agregar seus dados de séries temporais é uma decisão
importante e depende da história que você deseja contar com seus dados.
É comum usar diferentes agregações para visualizar os mesmos dados de séries
temporais de maneiras diferentes.

## Séries temporais e monitoramento

Na área de TI, os dados de séries temporais são frequentemente coletados para
monitorar itens como infraestrutura, hardware ou eventos de aplicações.
Os dados de séries temporais gerados por máquinas são normalmente coletados em
intervalos curtos, o que permite reagir a quaisquer mudanças inesperadas logo
após elas ocorrerem.
Consequentemente, os dados se acumulam rapidamente, tornando vital ter uma
maneira de armazená-los e consultá-los com eficiência.
Como resultado, os bancos de dados otimizados para dados de séries temporais têm
ganhado popularidade nos últimos anos.

### Bancos de dados de séries temporais

Um banco de dados de séries temporais (TSDB) é um banco de dados projetado
especificamente para dados de séries temporais.
Embora seja possível usar qualquer banco de dados comum para armazenar medições,
um TSDB oferece algumas otimizações úteis.

Os bancos de dados de séries temporais modernos aproveitam que as medições são
apenas adicionadas e raramente atualizadas ou removidas.
Por exemplo, os timestamps de cada medição mudam muito pouco ao longo do tempo,
o que resulta no armazenamento de dados redundantes.

Observe esta sequência de timestamps Unix:

```
1572524345, 1572524375, 1572524404, 1572524434, 1572524464
```

Observando esses timestamps, todos começam com `1572524`, o que leva a um uso
ineficiente do espaço em disco.
Em vez disso, poderíamos armazenar cada timestamp subsequente como a diferença,
ou _delta_, em relação ao primeiro:

```
1572524345, +30, +29, +30, +30
```

Poderíamos ir ainda mais longe, calculando os deltas desses deltas:

```
1572524345, +30, -1, +1, +0
```

Se as medições forem feitas em intervalos regulares, a maioria desses deltas de
deltas será 0.
Devido a otimizações como essas, os TSDBs usam drasticamente menos espaço do que
outros bancos de dados.

Outro recurso de um TSDB é a capacidade de filtrar medições usando _tags_.
Cada ponto de dados é rotulado com uma tag que adiciona informações contextuais,
como o local onde a medição foi feita.
Aqui está um exemplo do
[formato de dados do InfluxDB](https://docs.influxdata.com/influxdb/v1.7/write_protocols/line_protocol_tutorial/#syntax)
que demonstra como cada medição é armazenada.

```
weather,location=us-midwest temperature=82 1465839830100400200
  |    -------------------- --------------  |
  |             |             |             |
  |             |             |             |
+-----------+--------+-+---------+-+---------+
|measurement|,tag_set| |field_set| |timestamp|
+-----------+--------+-+---------+-+---------+
```

Aqui estão alguns dos bancos de dados de tipo único (TSDBs) suportados pelo
Grafana:

- [Graphite](https://graphiteapp.org/)
- [InfluxDB](https://www.influxdata.com/products/influxdb-overview/)
- [Prometheus](https://prometheus.io/)

### Coletando dados de séries temporais

Agora que temos um local para armazenar nossas séries temporais, como coletamos
as medições?
Para coletar dados de séries temporais, você normalmente instalaria um _coletor_
no dispositivo, máquina ou instância que deseja monitorar.
Alguns coletores são projetados para um banco de dados específico, enquanto
outros suportam diferentes destinos de saída.

Aqui estão alguns exemplos de coletores:

- [collectd](https://collectd.org/)
- [statsd](https://github.com/statsd/statsd)
- [Prometheus exporters](https://prometheus.io/docs/instrumenting/exporters/)
- [Telegraf](https://github.com/influxdata/telegraf)

Um coletor faz o _push_ (envia) dados para um banco de dados ou permite que o
banco de dados faça o _pull_ (extraia) os dados dele.
Ambos os métodos têm suas próprias vantagens e desvantagens:

|      | Vantagens                                                                  | Desvantagens                                                                     |
| ---- |----------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Push | Facilita a replicação de dados para múltiplos destinos.                    | O TSDB não tem controle sobre a quantidade de dados enviados.                    |
| Pull | Melhor controle sobre a quantidade de dados ingeridos e sua autenticidade. | Firewalls, VPNs ou balanceadores de carga podem dificultar o acesso aos agentes. |

Como seria ineficiente gravar cada medição no banco de dados, os coletores
pré-agregam os dados e os gravam no banco de dados de séries temporais em
intervalos regulares.
