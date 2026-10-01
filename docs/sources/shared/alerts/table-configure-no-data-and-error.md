---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

labels:
  products:
    - oss
title: 'Table configure no data and error'
---

| Configure        | Set alert state | Description                                                                                                                                                                                                                                                   |
| ---------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| No Data          | No Data         | The default option for **No Data** events.<br/>Sets alert instance state to `No Data`. <br/> The alert rule immediately creates a new `DatasourceNoData` alert instance after evaluation, with the alert rule's name, UID, and the data source UID as labels. |
| Error            | Error           | The default option for **Error** events.<br/>Sets alert instance state to `Error`. <br/> The alert rule immediately creates a new `DatasourceError` alert instance after evaluation, with the alert rule's name, UID, and the data source UID as labels.      |
| No Data or Error | Alerting        | Sets the alert instance state to `Pending` and then transitions to `Alerting` once the pending period ends. If you sent the pending period to 0, the alert instance state is immediately set to `Alerting`.                                                   |
| No Data or Error | Normal          | Sets alert instance state to `Normal`.                                                                                                                                                                                                                        |
| No Data or Error | Keep Last State | Maintains the alert instance in its last state. Useful for mitigating temporary issues.                                                                                                                                                                       |
