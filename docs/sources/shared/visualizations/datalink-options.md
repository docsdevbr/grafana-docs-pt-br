---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

title: Data links and actions options
comments: |
  There are four data link shared files, datalink-options.md, datalink-options-1.md, datalink-options-2.md, and datalink-options-3.md to cover the most common combinations of options.
  Using shared files ensures that content remains consistent across visualizations that share the same options and users don't have to figure out which options apply to a specific visualization when reading that content.
  This file is used in the following visualizations: geomap, heatmap, histogram
---

Data links allow you to link to other panels, dashboards, and external resources while maintaining the context of the source panel.
You can create links that include the series name or even the value under the cursor.

{{< admonition type="note" >}}
Actions are not supported for this visualization.
{{< /admonition >}}

For each data link, set the following options:

- **Title**
- **URL**
- **Open in new tab**

To learn more, refer to [Configure data links and actions](https://grafana.com/docs/grafana/<GRAFANA_VERSION>/panels-visualizations/configure-data-links/).
