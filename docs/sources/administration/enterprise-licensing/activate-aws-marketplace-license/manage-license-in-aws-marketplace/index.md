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
  - ../../../enterprise/activate-aws-marketplace-license/manage-license-in-aws-marketplace/
  - ../../../enterprise/license/activate-aws-marketplace-license/manage-license-in-aws-marketplace/
description: Manage your Grafana Enterprise license in AWS Marketplace
keywords:
  - grafana
  - enterprise
  - aws
  - marketplace
  - manage
  - add
  - remove
  - users
labels:
  products:
    - enterprise
    - oss
title: Manage your Grafana Enterprise license in AWS Marketplace
weight: 400
---

## Manage your Grafana Enterprise license in AWS Marketplace

You can use AWS Marketplace to make the following modifications to your Grafana Enterprise license:

- Add active users
- Remove active users
- Cancel your subscription to Grafana Enterprise.

**To modify your Grafana Enterprise subscription in AWS Marketplace:**

1. Open the AWS Console and navigate to [Subscription Management](https://console.aws.amazon.com/marketplace/home/subscriptions#/subscriptions).

1. Update your license.

1. Sign in to Grafana as a Server Administrator.

1. Click **Administration** in the side navigation menu, **General**, and then **Stats and license**.

1. In the **Token** section under **Enterprise License**, click **Renew token**.

   This action retrieves updated license information from AWS.

> To learn more about licensing and active users, refer to [Activate a Grafana Enterprise license purchased through AWS Marketplace](../).
