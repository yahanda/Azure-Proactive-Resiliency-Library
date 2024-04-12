+++
title = "Virtual Networks"
description = "Best practices and resiliency recommendations for Virtual Networks and associated resources."
date = "4/4/23"
author = "CHANGE ME TO YOUR GITHUB USERNAME"
msAuthor = "maheshbenke"
draft = false
+++

The presented resiliency recommendations in this guidance include Virtual Networks and associated Virtual Networks settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for Virtual Networks and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                                                          |     Category      | Impact |  State  | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [VNET-1 - All Subnets should have a Network Security Group associated](#vnet-1---all-subnets-should-have-a-network-security-group-associated)                                                                                                           | Access & Security |  High  | Preview |         Yes         |
| [VNET-2 - Use Azure DDoS Standard Protection Plans to protect all public endpoints hosted within customer Virtual Networks](#vnet-2---use-azure-ddos-standard-protection-plans-to-protect-all-public-endpoints-hosted-within-customer-virtual-networks) | Access & Security |  High  | Preview |         Yes         |
| [VNET-3 - Use Private Link, when available, for shared Azure PaaS services](#vnet-3---when-available-use-private-endpoints-instead-of-service-endpoints-for-paas-services)                                                                              | Access & Security | Medium | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### VNET-1 - すべてのサブネットにネットワーク セキュリティ グループが関連付けられている必要があります

**Category: Access & Security**

**Impact: High**

**Guidance**

ネットワーク・セキュリティ・グループ: ネットワーク・セキュリティ・グループおよびアプリケーション・セキュリティ・グループには、ソースおよび宛先のIPアドレス、ポートおよびプロトコルによってリソースとの間のトラフィックをフィルタリングできる複数の受信および送信セキュリティ規則を含めることができます。NSG は、サブネット レベルのセキュリティ層を提供します。GatewaySubnet、AzureFirewallSubnet、AzureFirewallManagementSubnet、RouteServerSubnet への NSG の適用はサポートされていないため、これらのサブネットは除外 (無視) されることに注意してください。

**Resources**

- [Azure Virtual Network - Concepts and best practices | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/virtual-network/concepts-and-best-practices)
- [GatewaySUbnet](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsub)
- [Can I associate a network security group (NSG) to the RouteServerSubnet?](https://learn.microsoft.com/ja-jp/azure/route-server/route-server-faq#can-i-associate-a-network-security-group-nsg-to-the-routeserversubnet)
- [Are Network Security Groups (NSGs) supported on the AzureFirewallSubnet?](https://learn.microsoft.com/ja-jp/azure/firewall/firewall-faq#are-network-security-groups--nsgs--supported-on-the-azurefirewallsubnet)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vnet-1/vnet-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VNET-2 - Azure DDoS Standard Protection プランを使用して、お客様の仮想ネットワーク内でホストされているすべてのパブリック エンドポイントを保護します

**Category: Access & Security**

**Impact: High**

**Guidance**

Azure DDoS Protection は、アプリケーション設計のベスト プラクティスと組み合わせることで、DDoS 攻撃から保護するための強化された DDoS 軽減機能を提供します。これは、仮想ネットワーク内の特定の Azure リソースを保護するのに役立つように自動的に調整されます。

**Resources**

- [Reliability and Azure Virtual Network - Microsoft Azure Well-Architected Framework | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/architecture/framework/services/networking/azure-virtual-network/reliability)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vnet-2/vnet-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VNET-3 - 使用可能な場合は、PaaS サービスにサービス エンドポイントではなくプライベート エンドポイントを使用します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

仮想ネットワーク サービス エンドポイントは、Private Link が使用できず、データの不正な移動の懸念がない場合にのみ使用します。VNet サービス エンドポイント機能 (ネットワーク側で VNet サービス エンドポイントを有効にし、Azure サービス側で適切な VNet ACL を設定する) により、許可された VNet とサブネットへの Azure サービス アクセスが制限されるため、ネットワーク レベルのセキュリティと Azure サービス トラフィックの分離が提供されます。VNet サービス エンドポイントを使用するすべてのトラフィックは Microsoft のバックボーンを経由するため、パブリック インターネットから分離する別のレイヤーが提供されます

**Resources**

- [Azure Virtual Network FAQ | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/virtual-network/virtual-networks-faq)
- [Reliability and Network connectivity - Microsoft Azure Well-Architected Framework | Microsoft LearnNetworking Reliability](https://learn.microsoft.com/ja-jp/azure/architecture/framework/services/networking/network-connectivity/reliability)
- [Azure Private Link availability](https://learn.microsoft.com/ja-jp/azure/private-link/availability)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vnet-3/vnet-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
