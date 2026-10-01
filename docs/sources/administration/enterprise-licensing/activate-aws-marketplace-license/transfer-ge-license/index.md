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
  - ../../../enterprise/activate-aws-marketplace-license/transfer-ge-license/
  - ../../../enterprise/license/activate-aws-marketplace-license/transfer-ge-license/
description: Transfer your AWS Marketplace Grafana Enterprise license
keywords:
  - grafana
  - enterprise
  - aws
  - marketplace
  - transfer
  - move
labels:
  products:
    - enterprise
    - oss
title: Transfer your AWS Marketplace Grafana Enterprise license
weight: 400
---

# Transfer your AWS Marketplace Grafana Enterprise license

You can transfer your AWS Marketplace Grafana Enterprise license to another Grafana Enterprise instance. The transfer process requires that you first remove your license from one instance, and then apply the license to another instance.

> When you remove an Enterprise license, the system immediately disables all Grafana Enterprise features.

To remove an Enterprise license from a Grafana Enterprise instance, perform one of the following steps:

- If you are using Amazon ECS or Amazon EKS, remove the `GF_ENTERPRISE_LICENSE_VALIDATION_TYPE` environment variable from the container.
- If you have deployed Grafana Enterprise outside of AWS, remove the `aws` license_validation_type value from the grafana.ini configuration file.

It can take the system up to one hour to clear the license. After the system clears the license, you can apply the same license to another Grafana Enterprise instance.

To determine that the system has returned your license, check the license details in AWS License Manager.
