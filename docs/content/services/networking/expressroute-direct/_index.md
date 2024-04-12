+++
title = "ExpressRoute Direct"
description = "Best practices and resiliency recommendations for ExpressRoute Direct and associated resources and settings."
date = "1/28/24"
author = "ehaslett"
msAuthor = "ethaslet"
draft = false
+++

The presented resiliency recommendations in this guidance include ExpressRoute Direct and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------: | :------: | :-----------------: |
| [ERD-1 - The Admin State of both Links of an ExpressRoute Direct should be in Enabled state](#erd-1---the-admin-state-of-both-links-of-an-expressroute-direct-should-be-in-enabled-state) | Availability | High | Verified | No |
| [ERD-2 - Ensure you do not over-subscribe an ExpressRoute Direct](#erd-2---ensure-you-do-not-over-subscribe-an-expressroute-direct) | System Efficiency | High | Verified | No |
| [ERD-3 - Enable rate-limiting to help optimize network performance by controlling the traffic volume across all your ExpressRoute Direct based circuits - In Preview](#erd-3---enable-rate-limiting-to-help-optimize-network-performance-by-controlling-the-traffic-volume-across-all-your-expressroute-direct-based-circuits---in-preview) | System Efficiency | Medium | Verified| No |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ERD-1 - ExpressRoute Direct の両方のリンクの管理状態が有効状態になっている必要があります

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

Azure ExpressRoute Direct では、"管理状態" は ExpressRoute レイヤー 1 リンクの管理状態を指します。これは基本的に、特定のリンクが有効か無効か、つまり物理ポートがオンかオフかを示します。ExpressRoute Direct 接続経由でトラフィックを渡すために必要です。管理状態は、ExpressRoute Direct の動作状態を決定し、オンプレミス ネットワークと Azure サービス間の接続に影響を与えるため、重要な設定です。

**Resources**

- [How to configure ExpressRoute Direct: Change Admin State of links](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-howto-erdirect#state)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erd-1/erd-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERD-2 - ExpressRoute Direct をオーバーサブスクライブしないことを確認します

**Category: System Efficiency**

**Impact: High**

**Recommendation/Guidance**

選択した ExpressRoute Direct リソース (10 Gbps または 100 Gbps) の上に、サブスクライブされた 20 Gbps または 200 Gbps の帯域幅まで、論理 ExpressRoute 回線をプロビジョニングできます。回復性の観点から、これは推奨されません。ExpressRoute Direct ポートの 1 つがダウンし、ExpressRoute 回線が既に 10 Gbps または 100 Gbps を 100% 消費している場合、2 番目の ExpressRoute Direct ポートには、追加の負荷をサポートするのに十分な帯域幅がありません。ポートがダウンする理由の 1 つは、メンテナンス イベント中です。残りのポートは、メンテナンス イベント中、最大 10 Gbps または 100 Gbps の容量のすべてのトラフィックをサポートします。ExpressRoute Direct 回線 (プレビュー) のレート制限を使用して非運用接続の帯域幅を制限しない限り、運用ワークロードに使用されている ExpressRoute Direct ポートをオーバーサブスクライブしないでください。

**Resources**

- [About ExpressRoute Direct: Circuit Sizes](https://learn.microsoft.com/ja-jp/azure/expressroute/expressroute-erdirect-about?source=recommendations#circuit-sizes)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erd-2/erd-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERD-3 - レート制限を有効にして、すべての ExpressRoute Direct ベースの回線のトラフィック量を制御することで、ネットワーク パフォーマンスを最適化します - プレビュー段階


**Category: System Efficiency**

**Impact: Medium**

**Recommendation/Guidance**

レート制限は、ExpressRoute Direct 回線を介してオンプレミス ネットワークと Azure 間のトラフィック量を制御できる機能です。これは、ExpressRoute 回線のプライベート ピアリングまたは Microsoft ピアリング経由のトラフィックに適用されます。この機能は、ポート帯域幅を回線間で均等に分散し、ネットワークの安定性を確保し、ネットワークの輻輳を防ぐのに役立ちます。このドキュメントでは、ExpressRoute Direct 回線のレート制限を有効にする手順について説明します。

**Resources**

- [Rate limiting for ExpressRoute Direct circuits (Preview)](https://learn.microsoft.com/ja-jp/azure/expressroute/rate-limit)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/erd-3/erd-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
