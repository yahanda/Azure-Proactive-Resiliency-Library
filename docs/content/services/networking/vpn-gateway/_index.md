+++
title = "VPN Gateway"
description = "Best practices and resiliency recommendations for VPN Gateway and associated resources."
date = "4/20/23"
author = "sitarant"
msAuthor = "sitarant"
draft = false
+++

The presented resiliency recommendations in this guidance include VPN Gateway and associated VPN Gateway settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for VPN Gateway and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                      |     Category      | Impact |  State  | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [VPNG-1 - Choose a Zone-redundant gateway](#vpng-1---choose-a-zone-redundant-gateway)                                                                               |   Availability    |  High  | Preview |         Yes         |
| [VPNG-2 - Plan for Active-Active mode](#vpng-2---plan-for-active-active-mode)                                                                                       |   Availability    |  High  | Preview |         Yes         |
| [VPNG-4 - Deploy active-active VPN concentrators on your premises for maximum resiliency](#vpng-4---deploy-active-active-vpn-concentrators-on-your-premises-for-maximum-resiliency) | Availability | High | Preview | No |                                                                | Availability |  Medium  | Preview |         No          |
| [VPNG-5 - Monitor connections and gateway health](#vpng-5---monitor-connections-and-gateway-health)                                                                 |    Monitoring     | Medium | Preview |         No          |
| [VPNG-6 - Enable service health](#vpng-6---enable-service-health)                                                                                                   |    Monitoring     | Medium | Preview |         No          |
| [VPNG-7 - Deploy zone-redundant VPN Gateways with zone-redundant Public IP(s)](#vpng-7---deploy-zone-redundant-vpn-gateways-with-zone-redundant-public-ips)         | Availability | Medium | Preview | Yes |                                                                                          |    Availability     | High | Preview |         Yes          |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### VPNG-1 - ゾーン冗長ゲートウェイを選択します

**Category: Availability**

**Impact: High**

**Guidance**

Azure VPN ゲートウェイは、1 つの可用性ゾーンにデプロイする場合と、2 つの可用性ゾーンにデプロイする場合で、異なる SLA を提供します。可用性ゾーン間で仮想ネットワーク ゲートウェイを自動的にデプロイするには、ゾーン冗長仮想ネットワーク ゲートウェイを使用できます。ゾーン冗長ゲートウェイを使用すると、ゾーン回復性を利用して、Azure 上のミッション クリティカルでスケーラブルなサービスにアクセスできます。

**Resources**

- [Zone redundant Virtual network gateway in availability zone](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/about-zone-redundant-vnet-gateways)
- [Gateway SKU](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/about-zone-redundant-vnet-gateways#gwskus)
- [SLA summary for Azure services](https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services?lang=1).

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-1/vpng-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-2 - アクティブ/アクティブモードを計画します

**Category: Availability**

**Impact: High**

**Guidance**

アクティブ/アクティブ モードは、Basic を除くすべての SKU で使用できます。
アクティブ/アクティブ ゲートウェイには、2 つのゲートウェイ IP 構成と 2 つのパブリック IP アドレスがあります。

**Resources**

- [Active-active VPN gateway](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/active-active-portal#gateway)
- [Gateway SKU](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-2/vpng-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-4 - 耐障害性を最大化するためアクティブ/アクティブ VPN コンセントレータをオンプレミスにデプロイします

**Category: Availability**

**Impact: High**

**Guidance**

アクティブ/アクティブ VPN コンセントレーターをアクティブ/アクティブ Azure VPN Gateway と共にオンプレミスにデプロイすることで、4 つの IPSec トンネルに基づくフルメッシュ トポロジを使用して、回復性と可用性を最大化できます。

**Resources**

- [Dual-redundancy: active-active VPN gateways for both Azure and on-premises networks](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/vpn-gateway-highlyavailable#dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks)


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-4/vpng-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-5 - 接続とゲートウェイの正常性を監視します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

使用可能なさまざまなメトリックに基づいて、Virtual Network ゲートウェイの正常性の監視とアラートを設定します。

**Resources**

- [VPN gateway data reference](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/monitor-vpn-gateway-reference)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-5/vpng-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-6 - サービス正常性を有効にします

**Category: Monitoring**

**Impact: Medium**

**Guidance**

VPN Gateway は、サービス正常性を使用して、計画メンテナンスと計画外のメンテナンスについて通知します。サービス正常性を構成すると、VPN 接続に加えられた変更について通知されます。

**Resources**

- [Getting started with Azure Metrics Explorer](hhttps://learn.microsoft.com/ja-jp/azure/azure-monitor/essentials/metrics-getting-started)
- [Monitor VPN gateway](hhttps://learn.microsoft.com/ja-jp/azure/vpn-gateway/monitor-vpn-gateway-reference#metrics)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-6/vpng-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-7 - ゾーン冗長パブリック IP を使用してゾーン冗長 VPN Gateway をデプロイします

**Category: Availability**

**Impact: High**

**Guidance**

VPN Gateway にゾーン冗長 SKU (VpnGw*AZ) を使用する場合は、ゲートウェイをゾーン冗長 Standard SKU パブリック IP アドレスに関連付けてください。VPN ゲートウェイがゾーンの Standard SKU パブリック IP アドレスに関連付けられている場合、すべてのゲートウェイ インスタンスは IP アドレスと同じゾーンにデプロイされます。この推奨事項は、アクティブ/パッシブ ゲートウェイ (1 つのパブリック IP アドレスを使用) とアクティブ/アクティブ VPN ゲートウェイ (2 つのパブリック IP アドレスを使用) の両方に適用されます。

**Resources**

- [About zone-redundant virtual network gateway in Azure availability zones](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/about-zone-redundant-vnet-gateways)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-7/vpng-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

