+++
title = "ExpressRoute Traffic Collector"
description = "Best practices and resiliency recommendations for ExpressRoute Traffic Collector and associated resources and settings."
date = "1/28/24"
author = "ehaslett"
msAuthor = "ethaslet"
draft = false
+++

The presented resiliency recommendations in this guidance include ExpressRoute Traffic Collector and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------: | :------: | :-----------------: |
| [ERTC-1 - Ensure ExpressRoute Traffic Collector is enabled and configured for ExpressRoute Direct circuits](#ertc-1---ensure-expressroute-traffic-collector-is-enabled-and-configured-for-expressroute-direct-circuits) | Monitoring | Medium | Preview | No |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ERTC-1 - ExpressRoute Traffic Collector が有効で、ExpressRoute Direct 回線用に構成されていることを確認します

**Category: Monitoring**

**Impact: Medium**

**Recommendation/Guidance**

ExpressRoute Traffic Collector を使用すると、ExpressRoute Direct 回線経由で送信されるネットワーク フローのサンプリングが可能になります。フロー ログは Log Analytics ワークスペースに送信され、そこで独自のログ クエリを作成してさらに分析できます。また、任意の視覚化ツールやSIEM(セキュリティ情報およびイベント管理)にデータをエクスポートすることもできます。フロー ログは、ExpressRoute Traffic Collector を使用したプライベート ピアリングと Microsoft ピアリングの両方で有効にできます。

1 つの ExpressRoute Direct 回線を、特定の地政学的リージョン内の異なる Azure リージョンにデプロイされた複数の ExpressRoute トラフィック コレクターに関連付けることができます。ディザスター リカバリーと高可用性の計画の一環として、ExpressRoute Direct 回線を複数の ExpressRoute トラフィック コレクターに関連付けることをお勧めします。

**Resources**

- [Azure ExpressRoute Traffic Collector](https://learn.microsoft.com/ja-jp/azure/expressroute/traffic-collector)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ertc-1/ertc-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
