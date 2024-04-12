+++
title = "Service Bus"
description = "Best practices and resiliency recommendations for Service Bus and associated resources and settings."
date = "2/13/24"
author = "DaFitRobsta"
msAuthor = "rolightn"
draft = false
+++

The presented resiliency recommendations in this guidance include Service Bus and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |                                Category                                 |     Impact      |      State       | ARG Query Available |
|:--------------------------------------------------|:-----------------------------------------------------------------------:|:---------------:|:----------------:|:-------------------:|
| [SBNS-1 - Enable Availability Zones for Service Bus namespaces](#sbns-1---enable-availability-zones-for-service-bus-namespaces) | Availability | High | Preview |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### SBNS-1 - Service Bus 名前空間の可用性ゾーンを有効にします

**Category: Availability**

**Impact: High**

**Guidance**

運用環境のワークロードには、ゾーン冗長性を備えた Service Bus を使用します。Service Bus Premium SKU では、可用性ゾーンがサポートされており、同じ Azure リージョン内に障害が分離された場所が提供されます。Service Bus は、メッセージング ストアの 3 つのコピー (1 つのプライマリと 2 つのセカンダリ) を管理します。Service Bus は、データ操作と管理操作のために 3 つのコピーすべての同期を維持します。プライマリ・コピーに障害が発生すると、セカンダリ・コピーの1つがプライマリ・コピーに昇格され、ダウンタイムは発生しません。アプリケーションで Service Bus からの一時的な切断が検出された場合、SDK の再試行ロジックによって Service Bus に自動的に再接続されます。

**Resources**

- [Service Bus and reliability](https://learn.microsoft.com/ja-jp/azure/well-architected/services/messaging/service-bus/reliability)
- [Azure Service Bus Geo-disaster recovery](https://learn.microsoft.com/ja-jp/azure/service-bus-messaging/service-bus-geo-dr#availability-zones)
- [Insulate Azure Service Bus applications against outages and disasters](https://learn.microsoft.com/ja-jp/azure/service-bus-messaging/service-bus-outages-disasters)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sbns-1/sbns-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
