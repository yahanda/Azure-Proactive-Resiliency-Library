+++
title = "ExpressRoute Gateway"
description = "Best practices and resiliency recommendations for ExpressRoute Gateway and associated resources."
date = "01/31/24"
author = "ehaslett"
msAuthor = "ethaslet"
draft = false
+++

The presented resiliency recommendations in this guidance include ExpressRoute Gateway and associated ExpressRoute Gateway settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------:|:------:|:-------:|:-------------------:|
| [ERGW-1 - Connect the ExpressRoute Gateway to two or more circuits from different peering locations for higher resiliency](#ergw-1---connect-the-expressroute-gateway-to-two-or-more-circuits-from-different-peering-locations-for-higher-resiliency) | Availability | High | Preview | No |
| [ERGW-2 - Use Zone-redundant gateway SKUs](#ergw-2---use-zone-redundant-gateway-skus) | Availability | High | Preview | Yes |
| [ERGW-3 - Configure an Azure Resource lock for ExpressRoute Gateway to prevent accidental deletion](#ergw-3---configure-an-azure-resource-lock-for-expressroute-gateway-to-prevent-accidental-deletion) | Availability | Medium | Preview | No |
| [ERGW-4 - Monitor gateway health](#ergw-4---monitor-gateway-health) | Monitoring | High | Preview | No |
| [ERGW-6 - Avoid using ExpressRoute circuits for VNet to VNet communication](#ergw-6---avoid-using-expressroute-circuits-for-vnet-to-vnet-communication) | Networking | Medium | Preview | No |
| [ERGW-7 - Configure customer-controlled gateway maintenance - In Preview](#ergw-7---configure-customer-controlled-gateway-maintenance---in-preview) | Networking | High | Preview | No |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ERGW-1 - 回復性を高めるために、ExpressRoute ゲートウェイを異なるピアリングの場所から 2 つ以上の回線に接続します

**Category: Availability**

**Impact: High**

**Guidance**

各 ExpressRoute ゲートウェイを少なくとも 2 つの回線に接続し、各回線が他の回線と比較して多様なピアリングの場所から接続されるようにします。

**Resources**

- [Designing for disaster recovery with ExpressRoute private peering](https://learn.microsoft.com/ja-jp/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ergw-1/ergw-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERGW-2 - ゾーン冗長ゲートウェイ SKU を使用します

**Category: Availability**

**Impact: High**

**Guidance**

Azure ExpressRoute ゲートウェイは、1 つの可用性ゾーンにデプロイされる場合と、2 つ以上の可用性ゾーンにデプロイされる場合に、異なる SLA を提供します。すべての Azure SLA の詳細については、[Online Services のサービス レベル アグリーメント (SLA)](https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services?lang=1&year=2023) を参照してください。可用性ゾーン間で仮想ネットワーク ゲートウェイを自動的にデプロイするには、ゾーン冗長仮想ネットワーク ゲートウェイを使用できます。ゾーン冗長ゲートウェイを使用すると、ゾーン回復性の恩恵を受けて、Azure 上のミッション クリティカルでスケーラブルなサービスにアクセスできます

**Resources**

- [About ExpressRoute virtual network gateways - Zone-redundant gateway SKUs](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-about-virtual-network-gateways#zrgw)
- [About zone-redundant virtual network gateway in Azure availability zones](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/about-zone-redundant-vnet-gateways)
- [Create a zone-redundant virtual network gateway in Azure Availability Zones](https://learn.microsoft.com/ja-jp/azure/vpn-gateway/create-zone-redundant-vnet-gateway)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ergw-2/ergw-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERGW-3 - 誤って削除されないように ExpressRoute ゲートウェイの Azure リソース ロックを構成します

**Category: Availability**

**Impact: Medium**

**Guidance**

ExpressRoute ゲートウェイの Azure リソース ロックを構成して、誤って削除されないようにします。管理者は、Azure サブスクリプション、リソース グループ、またはリソースをロックして、ユーザーが誤って削除したり変更したりしないように保護できます。ロックは、すべてのユーザー権限よりも優先されます。

**Resources**

- [Protect your Azure resources with a lock - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/management/lock-resources?tabs=json)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ergw-3/ergw-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERGW-4 - ゲートウェイの正常性を監視します

**Category: Monitoring**

**Impact: High**

**Guidance**

ExpressRoute ゲートウェイの可用性、パフォーマンス、スケーラビリティのために Network Insights を使用して監視を設定します。

アドバタイズされたルート、学習されたルート、VM の数の可用性メトリックのアラートを、使用中の ExpressRoute ゲートウェイ SKU でサポートされている量に基づいて構成します。お客様の環境に基づいて変更されるルートの頻度に関するアラートを構成します。

[ExpressRoute Gateways |Azure Monitor ベースライン アラート](https://azure.github.io/azure-monitor-baseline-alerts/services/Network/expressRouteGateways/) に従って、使用中の ExpressRoute ゲートウェイ SKU でサポートされている量とお客様の環境に基づいて、パケット/秒のアラートを構成します。

使用中の ExpressRoute ゲートウェイ SKU でサポートされている量とお客様環境で予想されるフロー数、およびこの値がお客様環境の履歴ベースラインを超えた場合の最大フロー数/秒に基づいて、アクティブなフローのスケーラビリティ メトリックのアラートを構成します。

**Resources**

- [ExpressRoute monitoring, metrics, and alerts | ExpressRoute gateways](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-monitoring-metrics-alerts#expressroute-gateways)
- [Azure ExpressRoute Insights using Network Insights](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-network-insights)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ergw-4/ergw-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERGW-6 - VNet 間通信に ExpressRoute 回線を使用しないようにします

**Category: Networking**

**Impact: Medium**

**Guidance**

既定では、複数の仮想ネットワーク (それぞれ ExpressRoute ゲートウェイ) を同じ ExpressRoute 回線にリンクすると、仮想ネットワーク間の接続が有効になります。ただし、仮想ネットワーク間の通信に ExpressRoute 回線を使用せず、代わりに VNet ピアリング、Azure Firewall、NVA、Azure Route Server 経由の VNet ハブでのルーティング、Azure 内のサイト間 VPN、仮想 WAN の使用、SD-WAN の使用など、他の手法を使用することをお勧めします。

ExpressRoute 経由の VNet 間接続が推奨されない理由の詳細については、[ExpressRoute 経由の仮想ネットワーク間の接続 | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/expressroute/virtual-network-connectivity-guidance) を参照ください。

**Resources**

- [About ExpressRoute virtual network gateways - VNet-to-VNet connectivity](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-about-virtual-network-gateways#vnet-to-vnet-connectivity)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ergw-6/ergw-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERGW-7 - お客様が管理するゲートウェイ メンテナンスを構成します - プレビュー段階

**Category: Availability**

**Impact: High**

**Guidance**

ExpressRoute 仮想ネットワーク ゲートウェイは、機能、信頼性、パフォーマンス、セキュリティを強化するために定期的に更新されます。お客様が管理するメンテナンスを構成してスケジュールすると、これらの更新の影響が最小限に抑えられ、メンテナンス期間に最も適した更新スケジュールが調整されます。

**Resources**

- [Configure customer-controlled maintenance for your virtual network gateway - ExpressRoute | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/expressroute/customer-controlled-gateway-maintenance#azure-portal-steps)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ergw-7/ergw-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
