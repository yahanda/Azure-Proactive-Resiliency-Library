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
| [VPNG-3 - Plan for Site-to-Site VPN and Azure ExpressRoute coexisting connection](#vpng-3---plan-for-site-to-site-vpn-and-azure-expressroute-coexisting-connection) | Disaster Recovery |  High  | Preview |         No          |
| [VPNG-4 - Plan for geo-redundant VPN Connections](#vpng-4---plan-for-geo-redundant-vpn-connections)                                                                 | Disaster Recovery |  High  | Preview |         No          |
| [VPNG-5 - Monitor connections and gateway health](#vpng-5---monitor-connections-and-gateway-health)                                                                 |    Monitoring     | Medium | Preview |         No          |
| [VPNG-6 - Enable service health](#vpng-6---enable-service-health)                                                                                                   |    Monitoring     | Medium | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### VPNG-1 - ゾーン冗長ゲートウェイを選択します

**Category: Availability**

**Impact: High**

**Guidance**

Azure VPN ゲートウェイは、1 つの可用性ゾーンにデプロイする場合と、2 つ以上の可用性ゾーンにデプロイする場合で、異なる SLA を提供します。すべての Azure SLA の詳細については、[Azure サービスの SLA の概要](https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services?lang=1) を参照してください。
可用性ゾーン間で仮想ネットワーク ゲートウェイを自動的にデプロイするには、ゾーン冗長仮想ネットワーク ゲートウェイを使用できます。ゾーン冗長ゲートウェイを使用すると、ゾーン回復性を利用して、Azure 上のミッション クリティカルでスケーラブルなサービスにアクセスできます。

**Resources**

- [Zone redundant Virtual network gateway in availability zone](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/about-zone-redundant-vnet-gateways)
- [Gateway SKU](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/about-zone-redundant-vnet-gateways#gwskus)

**Resource Graph Query/Scripts**

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

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-2/vpng-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-3 - サイト間 VPN と Azure ExpressRoute の共存接続を計画します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

最初の計画フェーズでは、ExpressRoute 接続を構成するかどうかを決定する必要があります。
Azure ExpressRoute 回線は、Azure へのプライベート専用接続を提供します。また、ビジネス ニーズに合った帯域幅と SKU の種類の要件を特定する必要もあります。ExpressRoute のフェイルオーバー・パスとしてのサイト間VPNを構成します。

**Resources**

- [Configure a Site-to-Site VPN as a failover path for ExpressRoute](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-howto-coexist-resource-manager#configuration-designs)
- [Limit and limitations](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-howto-coexist-resource-manager#limits-and-limitations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-3/vpng-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-4 - geo 冗長 VPN 接続を計画します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

ディザスタ・リカバリを計画するには、複数の場所でサイト間VPNを設定します。同じ都市圏または異なる都市圏で IP Sec 接続を作成し、さまざまなパスに対して異なるサービス プロバイダーと連携することを選択できます。

**Resources**

- [Highly available cross-premises](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/vpn-gateway-highlyavailable)
- [About VPN gateway redundancy](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/vpn-gateway-highlyavailable#about-vpn-gateway-redundancy)

**Resource Graph Query/Scripts**

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

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-5/vpng-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VPNG-6 - サービス正常性を有効化します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

VPN Gateway は、サービス正常性を使用して、計画メンテナンスと計画外のメンテナンスについて通知します。サービス正常性を構成すると、VPN 接続に加えられた変更について通知されます。

**Resources**

- [Getting started with Azure Metrics Explorer](hhttps://learn.microsoft.com/ja-jp/azure/azure-monitor/essentials/metrics-getting-started)
- [Monitor VPN gateway](hhttps://learn.microsoft.com/ja-jp/azure/vpn-gateway/monitor-vpn-gateway-reference#metrics)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vpng-6/vpng-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
