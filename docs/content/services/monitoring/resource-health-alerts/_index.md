+++
title = "Resource Health Alerts"
description = "Best practices and resiliency recommendations for Resources Health and associated resources and settings."
date = "11/6/23"
author = "anlucen"
msAuthor = "anlucena"
draft = false
+++

The presented resiliency recommendations in this guidance include Resources Health Alerts and associated resources and settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for Resources Health Alerts and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                        |  Category  | Impact |  State  | ARG Query Available |
|:--------------------------------------------------------------------------------------|:----------:|:------:|:-------:|:-------------------:|
| [MSR-1 - Configure Resource Health Alerts](#msr-1---configure-resource-health-alerts) | Monitoring |  Low   | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### MSR-1 - リソース正常性アラートを構成します

**Category: Monitoring**

**Impact: Low**

**Recommendation**

該当するすべてのリソースに対して Resource Health アラートを構成します。Azure Resource Health アラートは、Azure リソースの現在および過去の正常性状態に関する情報を提供します。Azure Resource Health アラートでは、これらのリソースの正常性状態に変化があったときに通知できます。

**Resources**

- [Resource Health](https://learn.microsoft.com/ja-jp/azure/service-health/resource-health-overview)
- [Configure Resource Health alerts in the Azure portal](https://learn.microsoft.com/ja-jp/azure/service-health/resource-health-alert-monitor-guide#create-a-resource-health-alert-rule-in-the-azure-portal)
- [Alerts Health](https://learn.microsoft.com/ja-jp/azure/service-health/alerts-activity-log-service-notifications-portal)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/msr-1/msr-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
