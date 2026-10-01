---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

title: Cell options
---

<!-- prettier-ignore-start -->
| Option | Description |
| ------ | ----------- |
| Cell value inspect | <p>Enables value inspection from table cells. When the switch is toggled on, clicking the inspect icon in a cell opens the **Inspect value** drawer which contains two tabs: **Plain text** and **Code editor**.</p><p>Grafana attempts to automatically detect the type of data in the cell and opens the drawer with the associated tab showing. However, you can switch back and forth between tabs.</p> |
| Tooltip from field | Toggle on the **Tooltip from field** switch to use the values from another field (or column) in a tooltip. For more information, refer to [Tooltip from field](#tooltip-from-field). |
| Styling from field | Toggle on the **Styling from field** switch to apply the styling from another field (or column). The referenced field must contain CSS properties formatted in JSON object syntax (for example, `{"name":"John"}`). For more information, refer to the [Styling from field](#styling-from-field). |
<!-- prettier-ignore-end -->
