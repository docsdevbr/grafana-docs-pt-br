---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

headless: true
labels:
  products:
    - enterprise
    - oss
---

#### Macros

You can enable macros support in the select clause to create time-series queries.

Use the **Data operations** drop-down to select a macro like `$__timeGroup` or `$__timeGroupAlias`.
Select a time column from the **Column** drop-down and a time interval from the **Interval** drop-down to create a time-series query.

{{< figure src="/media/docs/grafana/data-sources/screenshot-sql-builder-time-series-query.png" class="docs-image--no-shadow" caption="SQL query builder time-series query" >}}

You can also add custom value to the **Data operations**.
For example, a function that's not in the drop-down list.
This allows you to add any number of parameters.
