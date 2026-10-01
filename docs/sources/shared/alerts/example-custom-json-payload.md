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
    - cloud
    - enterprise
    - oss
title: 'Custom webhook example'
---

```go
{- /* Generates a pretty-printed JSON payload with alert group info and individual alerts metadata. */ -}}
{{ define "webhook.custom.payload" -}}
  {{ coll.Dict
  "receiver" .Receiver
  "status" .Status
  "alerts" (tmpl.Exec "webhook.custom.simple_alerts" .Alerts | data.JSON)
  "groupLabels" .GroupLabels
  "commonLabels" .CommonLabels
  "commonAnnotations" .CommonAnnotations
  "externalURL" .ExternalURL
  "version" "1"
  "orgId"  (index .Alerts 0).OrgID
  "truncatedAlerts"  .TruncatedAlerts
  "groupKey" .GroupKey
  "state"  (tmpl.Inline "{{ if eq .Status \"resolved\" }}ok{{ else }}alerting{{ end }}" . )
  "allVariables"  .Vars
  "title" (tmpl.Exec "default.title" . )
  "message" (tmpl.Exec "default.message" . )
  | data.ToJSONPretty " "}}
{{- end }}

{{- /* Embed json templates in other json templates. */ -}}
{{ define "webhook.custom.simple_alerts" -}}
  {{- $alerts := coll.Slice -}}
  {{- range . -}}
    {{ $alerts = coll.Append (coll.Dict
    "status" .Status
    "labels" .Labels
    "startsAt" .StartsAt
    "endsAt" .EndsAt
    ) $alerts}}
  {{- end -}}
  {{- $alerts | data.ToJSON -}}
{{- end }}
```
