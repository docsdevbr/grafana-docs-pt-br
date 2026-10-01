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
  - ../../../auth/grafana/ # /docs/grafana/next/auth/grafana/
  - ../../configure-security/setup-grafana/configure-security/configure-authentication/grafana/ # /docs/grafana/next/setup-grafana/configure-security/setup-grafana/configure-security/configure-authentication/grafana/
  - ../../configure-security/configure-authentication/grafana/ # /docs/grafana/next/setup-grafana/configure-security/configure-authentication/grafana/
description: Learn how to configure basic authentication in Grafana
labels:
  products:
    - enterprise
    - oss
menuTitle: Basic auth
title: Configure basic authentication
weight: 200
---

# Configure basic authentication

Grafana provides a basic authentication system with password authentication enabled by default. This document details configuration options to manage and enhance basic authentication.

## Disable basic authentication

To disable basic authentication, use the following configuration:

```bash
[auth.basic]
enabled = false
```

## Password policy

By default, Grafana’s password policy requires a minimum of four characters for basic auth users. For a stronger password policy, enable the `password_policy` configuration option.

With the `password_policy` option enabled, new and updated passwords must meet the following criteria:

- At least 12 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character

```bash
[auth.basic]
password_policy = true
```

{{< admonition type="note" >}}
Existing passwords that do not comply with the new password policy will not be affected until the user updates their password.
{{< /admonition >}}

## Disable login form

To hide the Grafana login form, use the following configuration setting:

```bash
[auth]
disable_login_form = true
```

This can be helpful in setups where authentication is handled entirely through external mechanisms or single sign-on (SSO).
