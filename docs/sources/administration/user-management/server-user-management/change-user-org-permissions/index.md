---
# SPDX-FileCopyrightText: 2026 Grafana Labs.
# Grafana and the Grafana logo are trademarks owned by Raintank, Inc. dba
# Grafana Labs.
#
# SPDX-License-Identifier: AGPL-3.0-only
# Documentation licensed under the GNU Affero General Public License Version 3.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/grafana-docs-pt-br/blob/-/LICENSES/AGPL-3.0-only.txt

aliases:
  - ../../manage-users-and-permissions/manage-server-users/change-user-org-permissions/
labels:
  products:
    - enterprise
    - oss
title: Change a user's organization permissions
weight: 50
---

# Change a user's organization permissions

Update organization permissions when you want to enhance or restrict a user's access to organization resources. For more information about organization permissions, refer to [Organization roles](../../../roles-and-permissions/#organization-roles).

## Before you begin

- [Add a user to an organization](../add-remove-user-to-org/)
- Ensure you have Grafana server administrator privileges

**To change a user's organization permissions**:

1. Sign in to Grafana as a server administrator.
1. Click **Administration** in the left-side menu, **Users and access**, and then **Users**.
1. Click a user.
1. In the Organizations section, click **Change role** for the role you want to change
1. Select another role.
1. Click **Save**.

{{< admonition type="note" >}}
In order for the change to take effect and be reflected within the instance, the account where permissions were altered will need to sign out fully and back in. Role assignment is evaluated during sign in, so if a user has not signed back in after their role was adjusted the instance will continue to reflect their previous role.
{{< /admonition >}}
