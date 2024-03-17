+++
title = "SignalR"
description = "Best practices and resiliency recommendations for SignalR and associated resources and settings."
date = "10/3/23"
author = "ejhenry"
msAuthor = "ejhenry"
draft = false
+++

The presented resiliency recommendations in this guidance include SignalR and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State            | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------:          | :-----------------: |
| [SIGR-1 - Enable zone redundancy for SignalR](#sigr-1---enable-zone-redundancy-for-signalr) | High Availability | High | Preview  |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### SIGR-1 - SignalR のゾーン冗長性を有効にします

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

運用ワークロードのゾーン冗長性で SignalR を使用します。ゾーン冗長は Premium レベルの機能です。これは、Premium レベルのリソースを作成またはアップグレードするときに暗黙的に有効になります。Standard レベルのリソースは、ダウンタイムなしで Premium レベルにアップグレードできます。

**Resources**

- [Availability zones support in Azure SignalR Service](https://learn.microsoft.com/ja-jp/azure/azure-signalr/availability-zones)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sigr-1/sigr-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
