+++
title = "Redis Cache"
description = "Best practices and resiliency recommendations for Redis Cache and associated resources and settings."
date = "10/2/23"
author = "ejhenry"
msAuthor = "ejhenry"
draft = false
+++

The presented resiliency recommendations in this guidance include Redis Cache and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State            | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------:          | :-----------------: |
| [REDIS-1 - Enable zone redundancy for Azure Cache for Redis](#redis-1---enable-zone-redundancy-for-azure-cache-for-redis) | High Availability | High | Preview  |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### REDIS-1 - Azure Cache for Redis のゾーン冗長を有効にします

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

Azure Cache for Redis では、Premium レベルと Enterprise レベルでゾーン冗長性がサポートされています。ゾーン冗長キャッシュは、複数の可用性ゾーンに分散した VM で実行されます。ゾーン冗長性により、回復性と可用性が向上します。

**Resources**

- [Enable zone redundancy for Azure Cache for Redis](https://learn.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-how-to-zone-redundancy)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/redis-1/redis-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
