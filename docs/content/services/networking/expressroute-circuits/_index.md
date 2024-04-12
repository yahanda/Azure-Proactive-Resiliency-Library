+++
title = "ExpressRoute Circuits"
description = "Best practices and resiliency recommendations for ExpressRoute circuits and associated resources."
date = "01/31/2024"
author = "ehaslett"
msAuthor = "ethaslet"
draft = false
+++

The presented resiliency recommendations in this guidance include ExpressRoute circuits and associated ExpressRoute circuit settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for ExpressRoute circuits and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                                                                    |     Category      | Impact |  State  | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [ERC-1 - Connect your on-premises network to critical workloads in Azure through two or more ExpressRoute circuits in different peering locations](#erc-1---connect-your-on-premises-network-to-critical-workloads-in-azure-through-two-or-more-expressroute-circuits-in-different-peering-locations) |   Availability    |  High  | Verified |         No          |
| [ERC-2 - Ensure the two physical links of your ExpressRoute circuit are connected to two distinct edge devices in your network](#erc-2---ensure-the-two-physical-links-of-your-expressroute-circuit-are-connected-to-two-distinct-edge-devices-in-your-network)   |   Availability    |  High  | Verified |         No          |
| [ERC-3 - Ensure both connections of an ExpressRoute circuit are configured in active-active mode](#erc-3---ensure-both-connections-of-an-expressroute-circuit-are-configured-in-active-active-mode)                                                               |   Availability    |  High  | Verified |         Yes         |
| [ERC-4 - Ensure Bidirectional Forwarding Detection is enabled and configured on customer or provider edge routing devices](#erc-4---ensure-bidirectional-forwarding-detection-is-enabled-and-configured-on-customer-or-provider-edge-routing-devices)             |   Availability    |  High  | Verified |         No          |
| [ERC-5 - Configure monitoring and alerting for ExpressRoute circuits](#erc-5---configure-monitoring-and-alerting-for-expressroute-circuits)                                                                                                                       |    Monitoring     | Medium | Verified |         No          |
| [ERC-6 - Configure service health to receive ExpressRoute circuit maintenance notification](#erc-6---configure-service-health-to-receive-expressroute-circuit-maintenance-notification)                                                                           |    Monitoring     | Medium | Verified |         No          |
| [ERC-7 - Use a site-to-site VPN as an interim backup solution for a single ExpressRoute circuit](#erc-7---use-a-site-to-site-vpn-as-an-interim-backup-solution-for-a-single-expressroute-circuit)                                                                 | Disaster Recovery | Medium | Verified |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ERC-1 - 異なるピアリングの場所にある 2 つ以上の ExpressRoute 回線を使用して、オンプレミス ネットワークを Azure の重要なワークロードに接続します

**Category: Availability**

**Impact: High**

**Guidance**

各 ExpressRoute ゲートウェイを、異なるピアリングの場所でインスタンス化された少なくとも 2 つの回線に接続します。

**Resources**

- [Designing for disaster recovery with ExpressRoute private peering](https://learn.microsoft.com/ja-jp/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erc-1/erc-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERC-2 - ExpressRoute 回線の 2 つの物理リンクがネットワーク内の 2 つの異なるエッジ デバイスに接続されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

Microsoft (ExpressRoute direct モデルの場合) または ExpressRoute プロバイダー (ExpressRoute プロバイダー ベース モデルの場合) は、常に物理的に冗長なサービスを提供します。ExpressRoute ピアリングの場所からネットワークまでのパス全体で、同じレベルの物理冗長性 (2 つの物理デバイス、2 つの物理リンク) が使用されていることを確認します。

**Resources**

- [Designing for high availability with ExpressRoute](https://learn.microsoft.com/ja-jp/azure/expressroute/designing-for-high-availability-with-expressroute)
- [Azure Well-Architected Framework review - Azure ExpressRoute - Design Checklist](https://learn.microsoft.com/ja-jp/azure/well-architected/services/networking/azure-expressroute#recommendations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erc-2/erc-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERC-3 - ExpressRoute 回線の両方の接続がアクティブ/アクティブ モードで構成されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

高可用性を向上させるには、ExpressRoute 回線の両方の接続をアクティブ/アクティブ モードで運用することをお勧めします。接続をアクティブ/アクティブ モードで動作するように構成すると、Microsoft ネットワークはフローごとに接続間でトラフィックの負荷を分散します。

**Resources**

- [Designing for high availability with ExpressRoute - Active-active connections](https://learn.microsoft.com/ja-jp/azure/expressroute/designing-for-high-availability-with-expressroute#active-active-connections)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erc-3/erc-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERC-4 - お客様またはプロバイダーのエッジ ルーティング デバイスで双方向フォワーディング検出が有効化および構成されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

ExpressRoute 経由の双方向フォワーディング検出 (BFD) を有効にすると、Microsoft エンタープライズ エッジ (MSEE) デバイスと、ExpressRoute 回線が構成されるルーター (CE/PE) 間のリンク障害検出を高速化できます。ExpressRoute は、エッジ ルーティング デバイスまたはパートナー エッジ ルーティング デバイス (マネージド レイヤー 3 接続サービスを使用した場合) を介して構成できます。

**Resources**

- [Configure BFD over ExpressRoute](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-bfd)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erc-4/erc-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERC-5 - ExpressRoute 回線の監視とアラートを構成します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

ExpressRoute 回線の可用性、回線の QoS、スループットに関する Network Insights を使用して監視を構成します。可用性メトリックと回線 QoS メトリックのアラートを [ExpressRoute Circuits |Azure Monitor ベースライン アラート](https://azure.github.io/azure-monitor-baseline-alerts/services/Network/expressRouteCircuits/)、およびビット/秒が ExpressRoute 回線 SKU とお客様の使用状況に適したしきい値を超えた場合のスループット メトリックに従って構成します。

ExpressRoute の接続モニターと Log Analytics ワークスペース、および Network Watcher を使用してアラートを構成します。ChecksFailedPercent が 5% を超えたとき、および RoundTripTimeMs が環境に適した事前テスト済みの平均を超えたときにアラートを構成します。

ExpressRoute Direct の場合は、フロー ログを Log Analytics ワークスペースに送信するように ExpressRoute Direct のトラフィック収集を構成します。

**Resources**

- [Azure ExpressRoute Insights using Network Insights | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-network-insights)
- [Monitoring Azure ExpressRoute](https://learn.microsoft.com/ja-jp/azure/expressroute/monitor-expressroute)
- [Configure Traffic Collector for ExpressRoute Direct - Azure ExpressRoute | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/expressroute/how-to-configure-traffic-collector#deploy-expressroute-traffic-collector)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erc-5/erc-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERC-6 - ExpressRoute 回線のメンテナンス通知を受信するようにサービス正常性を構成します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

ExpressRoute では、サービス正常性を使用して、計画メンテナンスと計画外のメンテナンスについて通知します。サービス正常性を構成すると、ExpressRoute 回線に加えられた変更について通知されます。

**Resources**

- [How to view and configure alerts for Azure ExpressRoute circuit maintenance](https://learn.microsoft.com/ja-jp/azure/expressroute/maintenance-alerts)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erc-6/erc-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERC-7 - 単一の ExpressRoute 回線の暫定バックアップ ソリューションとしてサイト間 VPN を使用します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

ExpressRoute ゲートウェイの 2 番目の ExpressRoute 回線をまだ追加していない場合は、2 番目の ExpressRoute 回線が使用可能になるまでの暫定的なソリューションとしてサイト間 VPN を使用します。

**Resources**

- [Using S2S VPN as a backup for ExpressRoute private peering](https://learn.microsoft.com/ja-jp/azure/expressroute/use-s2s-vpn-as-backup-for-expressroute-privatepeering)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erc-7/erc-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
