+++
title = "Service Health Alerts"
description = "Best practices and resiliency recommendations for Service Health Alerts and associated resources and settings."
date = "2/23/24"
author = "ejhenry"
msAuthor = "erhenry"
draft = false
+++

The presented resiliency recommendations in this guidance include Service Health Alerts and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |                                Category                                 |     Impact      |      State       | ARG Query Available |
|:--------------------------------------------------|:-----------------------------------------------------------------------:|:---------------:|:----------------:|:-------------------:|
| [ALA-1 - Configure Service Health Alerts](#ala-1---configure-service-health-alerts) | Monitoring | High | Preview |         No         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ALA-1 - サービス正常性アラートを構成します

**Category: Monitoring**

**Impact: High**

**Guidance**

サービス正常性は、使用している Azure サービスとリージョンの正常性を個人用に表示します。これは、認証された Service Health エクスペリエンスによって、現在使用しているサービスとリソースが認識されるため、停止、計画メンテナンス アクティビティ、その他の正常性に関するアドバイザリに関する通信に影響を与えるサービスを探すのに最適な場所です。Service Health を使用する最善の方法は、サービスの問題、計画メンテナンス、またはその他の変更が、使用する Azure サービスやリージョンに影響を与える可能性がある場合に、優先する通信チャネルを介して通知するように Service Health アラートを設定することです。

**Resources**

- [What is Azure Service Health?](https://learn.microsoft.com/ja-jp/azure/service-health/overview)
- [Configure alerts for service health events](https://learn.microsoft.com/ja-jp/azure/service-health/alerts-activity-log-service-notifications-portal)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ala-1/ala-1.kql" >}} {{< /code >}}

{{< /collapse >}}
