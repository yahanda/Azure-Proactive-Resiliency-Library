+++
title = "Event Hub"
description = "Best practices and resiliency recommendations for Event Hub and associated resources and settings."
date = "10/6/23"
author = "ejhenry"
msAuthor = "ejhenry"
draft = false
+++

The presented resiliency recommendations in this guidance include Event Hub and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State            | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------:          | :-----------------: |
| [EVHNS-1 - Enable zone redundancy for Event Hub namespace](#evhns-1---enable-zone-redundancy-for-event-hub-namespace) | High Availability | High | Preview  |         Yes         |
| [EVHNS-2 - Enable auto-inflate on Event Hub Standard tier](#evhns-2---enable-auto-inflate-on-event-hub-standard-tier) | System Efficiency | High | Preview | Yes |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### EVHNS-1 - Event Hub 名前空間のゾーン冗長を有効にします

**Category: Availability**

**Impact: High**

**Recommendation**

Event Hubs は Availability Zones をサポートしており、Azure リージョン内で障害が分離された場所を提供します。Availability Zones のサポートは、可用性ゾーンがある Azure リージョンでのみ使用できます。メタデータとデータ (イベント) の両方が、可用性ゾーン内のデータセンター間でレプリケートされます。

**Resources**

- [Azure Event Hubs - Geo-disaster recovery](https://learn.microsoft.com/ja-jp/azure/event-hubs/event-hubs-geo-dr?tabs=portal#availability-zones)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/evhns-1/evhns-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### EVHNS-2 - Event Hub Standard レベルで自動インフレを有効にします

**Category: System Efficiency**

**Impact: High**

**Recommendation**

Event Hub Standard レベルの名前空間で自動インフレを有効にします。Event Hubs の自動インフレ機能は、使用ニーズを満たすために TU の数を増やすことで自動的にスケールアップされます。TU を増やすと、データのイングレス レートまたはデータ送信レートが、名前空間に割り当てられた TU で許可されているレートを超える調整シナリオが防止されます。

**Resources**

- [Azure Event Hubs - Automatically scale throughput units](https://learn.microsoft.com/ja-jp/azure/event-hubs/event-hubs-auto-inflate)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/evhns-2/evhns-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
