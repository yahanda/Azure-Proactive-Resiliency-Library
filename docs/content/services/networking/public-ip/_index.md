+++
title = "Public Ip"
description = "Best practices and resiliency recommendations for Public Ip and associated resources."
date = "4/12/23"
author = "lachaves"
msAuthor = "luchaves"
draft = false
+++

The presented resiliency recommendations in this guidance include Public Ip and associated Public Ip settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for Public Ip and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                            |   Category   | Impact |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------|:------------:|:------:|:-------:|:-------------------:|
| [PIP-1 - Use Zone-Redundant IPs when applicable](#pip-1---use-standard-sku-and-zone-redundant-ips-when-applicable)                       | Availability |  High  | Preview |         Yes          |
| [PIP-2 - Use NAT gateway for outbound connectivity to avoid SNAT Exhaustion](#pip-2---use-nat-gateway-for-outbound-connectivity-to-avoid-snat-exhaustion) | Availability | Medium | Preview |         Yes         |
| [PIP-3 - Upgrade Basic SKU public IP addresses to Standard SKU](#pip-3---upgrade-basic-sku-public-ip-addresses-to-standard-sku)                           | Availability | Medium | Preview |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### PIP-1 - 該当する場合は Standard SKU とゾーン冗長 IP を使用します

**Category: Availability**

**Impact: High**

**Guidance**

Standard SKU のパブリック IP アドレスは、可用性ゾーンをサポートするリージョンで、非ゾーン、ゾーン、またはゾーン冗長として作成できます。
ゾーン冗長 IP は、リージョンのすべてのゾーンに作成され、1 つのゾーンで障害が発生しても存続できます。ゾーン IP は特定の可用性ゾーンに関連付けられており、ゾーンの正常性と運命を共有します。"ゾーン以外の" パブリック IP アドレスは、Azure によってゾーンに配置され、冗長性は保証されません。 ゾーンの回復性をサポートするリソース (Azure Load Balancer や Azure Firewall など) でパブリック IP を利用する場合は、ほとんどの場合、ゾーン冗長 IP を使用することをお勧めします。

**Resources**

- [Public IP addresses - Availability Zones](https://learn.microsoft.com/ja-jp/azure/virtual-network/ip-services/public-ip-addresses#availability-zone)
- [Upgrading a basic public IP address to Standard SKU](https://learn.microsoft.com/ja-jp/azure/virtual-network/ip-services/public-ip-basic-upgrade-guidance#steps-to-complete-the-upgrade)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/pip-1/pip-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### PIP-2 - 送信接続に NAT ゲートウェイを使用して、SNAT の枯渇を回避します

**Category: Availability**

**Impact: Medium**

**Guidance**

仮想ネットワークからの送信トラフィックに NAT ゲートウェイを使用して、SNAT ポートの枯渇による接続エラーのリスクを回避します。NAT ゲートウェイは動的にスケーリングされ、インターネットに向かうトラフィックに安全な接続を提供します。

**Resources**

- [Use NAT GW for outbound connectivity](https://learn.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations#use-nat-gateway-for-outbound-connectivity)
- [TCP and SNAT Ports](https://learn.microsoft.com/ja-jp/azure/architecture/framework/services/compute/azure-app-service/reliability#tcp-and-snat-ports)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/pip-2/pip-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### PIP-3 - Upgrade Basic SKU public IP addresses to Standard SKU

**Category: Availability**

**Impact: Medium**

**Guidance**

On September 30, 2025, Basic SKU public IP addresses will be retired. If you are currently using Basic SKU public IP addresses, make sure to upgrade to Standard SKU public IP addresses prior to the retirement date.

**Resources**

- [Upgrading a basic public IP address to Standard SKU - Guidance](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-basic-upgrade-guidance)
- [Upgrade to Standard SKU public IP addresses in Azure by 30 September 2025—Basic SKU will be retired](https://azure.microsoft.com/en-us/updates/upgrade-to-standard-sku-public-ip-addresses-in-azure-by-30-september-2025-basic-sku-will-be-retired/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/pip-3/pip-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
