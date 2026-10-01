---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

keywords:
  - Infrastructure as Code
  - Quickstart
  - Grafana Cloud
  - Ansible
menuTitle: Ansible
title: Grafana Ansible collection
weight: 110
canonical: https://grafana.com/docs/grafana/latest/as-code/infrastructure-as-code/ansible/
aliases:
  - ../../infrastructure-as-code/ansible/ansible-grafana-agent-linux
  - ../../infrastructure-as-code/ansible/ansible-multiple-agents
labels:
  products:
    - cloud
---

# Grafana Ansible collection

The [Grafana Ansible collection](https://docs.ansible.com/ansible/latest/collections/grafana/grafana/) provides configuration management resources for Grafana. You can use it to manage:

- Grafana Cloud stacks
- Dashboards
- Data sources
- Folders
- Alerting contact points
- Notification policies
- API keys

If your resources aren't currently available in the Grafana Ansible collection, you can manage them on Grafana Cloud programmatically by writing Ansible playbooks that use the [Ansible's built-in URI module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html) to call the [HTTP APIs](/docs/grafana/latest/developers/http_api/) to manage resources for the Grafana Cloud portal, as well as those within a stack.

## Learn more

Refer to [Create and manage a Grafana Cloud stack using Ansible](ansible-cloud-stack/) to learn how to create a Grafana Cloud stack and add a data source and dashboard using [Ansible](https://www.ansible.com/).

To learn more about managing Grafana with Infrastructure as code:

- [Grafana Ansible collection documentation](https://docs.ansible.com/ansible/latest/collections/grafana/grafana/)
- [Ansible playbook best practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)
- [Grafana API documentation](/docs/grafana/latest/developers/http_api/)
- [Grafana Cloud API documentation](https://grafana.com/docs/grafana-cloud/developer-resources/api-reference/)
- [Infrastructure as Code with Terraform](/docs/grafana/latest/as-code/infrastructure-as-code/terraform/)

## Grafana Agent (deprecated)

{{< docs/shared lookup="agent-deprecation.md" source="alloy" version="next" >}}

The Ansible collection also houses [Grafana Agent role](https://github.com/grafana/grafana-ansible-collection/tree/main/roles/grafana_agent), which is now deprecated.
