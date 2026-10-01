---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

description: A reference for the JSON library panel schema used with Observability as Code.
keywords:
  - configuration
  - as code
  - as-code
  - dashboards
  - git integration
  - git sync
  - github
  - library panel
labels:
  products:
    - cloud
    - enterprise
    - oss
menuTitle: LibraryPanelKind schema
title: LibraryPanelKind
weight: 300
---

# `LibraryPanelKind`

A library panel is a reusable panel that you can use in any dashboard.
When you make a change to a library panel, that change propagates to all instances of where the panel is used.
Library panels streamline reuse of panels across multiple dashboards.

Following is the default library panel element JSON:

```json
      "kind": "LibraryPanel",
      "spec": {
        "id": 0,
        "libraryPanel": {
          name: "",
          uid: "",
        }
        "title": ""
      }
```

The `LibraryPanelKind` consists of:

- kind: "LibraryPanel"
- spec: [LibraryPanelKindSpec](#librarypanelkindspec)
  - libraryPanel: [LibraryPanelRef](#librarypanelref)

## `LibraryPanelKindSpec`

The following table explains the usage of the library panel element JSON fields:

| Name         | Usage                                            |
| ------------ | ------------------------------------------------ |
| id           | Panel ID for the library panel in the dashboard. |
| libraryPanel | [`LibraryPanelRef`](#librarypanelref)            |
| title        | Title for the library panel in the dashboard.    |

### `LibraryPanelRef`

The following table explains the usage of the library panel reference JSON fields:

| Name | Usage              |
| ---- | ------------------ |
| name | Library panel name |
| uid  | Library panel uid  |
