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
title: 'Math example'
---

- `$A` returns series `{host="web01"} 30` and `{host="web02"} 20`.
- `$B` returns series `{host="web01"} 10` and `{host="web02"} 0`.
- `$A + $B` returns `{host="web01"} 40` and `{host="web02"} 20`.
