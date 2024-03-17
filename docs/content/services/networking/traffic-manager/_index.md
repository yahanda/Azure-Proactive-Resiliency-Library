+++
title = "Traffic Manager"
description = "Best practices and resiliency recommendations for Azure Traffic Manager."
date = "9/21/23"
author = "chinthakaru"
msAuthor = "crupasinghe"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure Traffic Manager and associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                            |     Category      | Impact |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [TRAF-1 - Traffic Manager Monitor Status Should be Online](#traf-1---traffic-manager-monitor-status-should-be-online)                                     |   Availability    |  High  | Preview |         No          |
| [TRAF-2 - Traffic manager profiles should have more than one endpoint](#traf-2---traffic-manager-profiles-should-have-more-than-one-endpoint)             |   Availability    |  High  | Preview |         No          |
| [TRAF-3 - Configure at least one endpoint within a another region](#traf-3---configure-at-least-one-endpoint-within-a-another-region)                     | Disaster Recovery | Medium | Preview |         No          |
| [TRAF-4 - TTL value of user profiles should be in 60 Seconds](#traf-4---ttl-value-of-user-profiles-should-be-in-60-seconds)                               | System Efficiency | Medium | Preview |         No          |
| [TRAF-5 - Ensure endpoint configured to (All World) for geographic profiles](#traf-5---ensure-endpoint-configured-to-all-world-for-geographic-profiles) | Disaster Recovery | Medium | Preview |         Yes         |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### TRAF-1 - Traffic Manager モニターの状態はオンラインにする必要があります

**Category: Availability**

**Impact: High**

**Guidance**

モニターの状態は、アプリケーション ワークロードのフェールオーバーを提供するためにオンラインである必要があります。 Traffic Manager の正常性に Degraded (低下) 状態が表示される場合は、1 つ以上のエンドポイントの状態が Degraded(低下) になっている可能性があります。

**Resources**

- [Azure Traffic Manager endpoint monitoring](https://learn.microsoft.com/ja-jp/azure/traffic-manager/traffic-manager-monitoring)
- [Enable or disable health checks](https://learn.microsoft.com/ja-jp/azure/traffic-manager/traffic-manager-monitoring#enable-or-disable-health-checks-preview)
- [Troubleshooting degraded state on Azure Traffic Manager](https://learn.microsoft.com/ja-jp/azure/traffic-manager/traffic-manager-troubleshooting-degraded)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}
{{< code lang="sql" file="code/traf-1/traf-1.kql" >}} {{< /code >}}
{{< /collapse >}}
<br><br>

### TRAF-2 - Traffic Manager プロファイルには複数のエンドポイントが必要です

**Category: Availability**

**Impact: High**

**Guidance**

Azure Traffic Manager を構成するときは、ワークロードを別のインスタンスにフェールオーバーするために、少なくとも 2 つのエンドポイントをプロビジョニングする必要があります。

**Resources**

- [Traffic Manager Endpoint Types](https://learn.microsoft.com/ja-jp/azure/traffic-manager/traffic-manager-endpoint-types)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/traf-2/traf-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### TRAF-3 - 別のリージョン内に少なくとも 1 つのエンドポイントを構成します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

プロファイルには、エンドポイントの 1 つに障害が発生した場合に可用性を確保するために、複数のエンドポイントが必要です。また、エンドポイントを異なるリージョンに配置することもお勧めします。

**Resources**

- [Reliability recommendations
](https://learn.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations#add-at-least-one-more-endpoint-to-the-profile-preferably-in-another-azure-region)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/traf-3/traf-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### TRAF-4 - ユーザープロファイルの TTL 値は 60 秒にする必要があります

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

Time to Live (TTL) は、クライアントが Azure Traffic Manager に要求を行ったときに取得する応答の最新度に影響します。TTL 値を減らすと、フェールオーバーの場合に、クライアントは機能しているエンドポイントに高速にルーティングされます。TTL を 60 秒に設定して、トラフィックを正常性エンドポイントにできるだけ早くルーティングします。

**Resources**

- [Configure DNS Time to Live to 60 seconds).](https://learn.microsoft.com/ja-jp/azure/advisor/advisor-reference-performance-recommendations#configure-dns-time-to-live-to-60-seconds)
- [Traffic Manager profile - ProfileTTL (Configure DNS Time to Live to 60 seconds).](https://aka.ms/Um3xr5)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/traf-4/traf-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### TRAF-5 - 地理的プロファイルのエンドポイントが すべて (世界) に構成されていることを確認します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

地理的ルーティングの場合、トラフィックは定義されたリージョンに基づいてエンドポイントにルーティングされます。リージョンに障害が発生した場合、事前定義されたフェールオーバーはありません。地域プロファイルの地域グループを「すべて(世界)」に構成したエンドポイントを使用すると、トラフィックのブラックホール化が回避され、サービスが引き続き利用可能であることが保証されます。

**Resources**

- [Add an endpoint configured to "All (World)"](https://learn.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations#add-an-endpoint-configured-to-all-world)
- [Traffic Manager profile - GeographicProfile (Add an endpoint configured to ""All (World)"").](https://aka.ms/Rf7vc5)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/traf-5/traf-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
