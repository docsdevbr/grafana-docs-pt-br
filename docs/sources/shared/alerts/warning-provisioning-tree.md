---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

title: 'Warning Provisioning Tree'
---

{{< admonition type="warning" >}}

Since the policy tree is a single resource, provisioning it will overwrite all policies in the notification policy tree. However, it does not affect internal policies created when alert rules directly select a contact point.

{{< /admonition >}}
