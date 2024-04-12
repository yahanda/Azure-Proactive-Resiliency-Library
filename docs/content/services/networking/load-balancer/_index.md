+++
title = "Load Balancer"
description = "Best practices and resiliency recommendations for Load Balancer and associated resources."
date = "10/19/23"
author = "lachaves"
msAuthor = "luchaves"
draft = false
+++

The presented resiliency recommendations in this guidance include Load Balancer and associated Load Balancer settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for Load Balancer and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                          |   Category   | Impact |  State  | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------|:------------:|:------:|:-------:|:-------------------:|
| [LB-1 - Use Standard Load Balancer SKU](#lb-1---use-standard-load-balancer-sku)                                                                         | Availability |  High  | Verified |         Yes         |
| [LB-2 - Ensure the Backend Pool contains at least two instances](#lb-2---ensure-the-backend-pool-contains-at-least-two-instances)                       | Availability |  High  | Verified |         Yes         |
| [LB-3 - Use NAT Gateway instead of Outbound Rules for Production Workloads](#lb-3---use-nat-gateway-instead-of-outbound-rules-for-production-workloads) | Availability | Medium | Verified |         Yes         |
| [LB-4 - Ensure Standard Load Balancer is zone-redundant](#lb-4---ensure-standard-load-balancer-is-zone-redundant)                                       | Availability |  High  | Verified |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### LB-1 - Standard Load Balancer SKU を使用します

**Category: Availability**

**Impact: High**

**Guidance**

Standard SKU を選択します。Standard Load Balancer は、Basic にはない信頼性の側面 (可用性ゾーンとゾーンの回復性) を提供します。つまり、ゾーンがダウンしても、ゾーン冗長 Standard Load Balancer は影響を受けません。これにより、デプロイはリージョン内のゾーン障害に耐えることができます。さらに、Standard Load Balancer ではグローバル負荷分散がサポートされているため、アプリケーションもリージョンの障害の影響を受けません。Basic ロード バランサーには、サービス レベル アグリーメント (SLA) がありません。

**Resources**

- [Reliability and Azure Load Balancer](https://learn.microsoft.com/ja-jp/azure/architecture/framework/services/networking/azure-load-balancer/reliability)
- [Resiliency checklist for specific Azure services- Azure Load Balancer](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#azure-load-balancer)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/lb-1/lb-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### LB-2 - バックエンド プールに少なくとも 2 つのインスタンスが含まれていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

バックエンドに少なくとも 2 つのインスタンスを含む Azure LB をデプロイします。インスタンスが 1 つあるだけで、単一障害点が発生する可能性があります。スケールに合わせてビルドするには、LB と Virtual Machine Scale Sets を組み合わせることができます。

**Resources**

- [Resiliency checklist for specific Azure services- Azure Load Balancer](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#azure-load-balancer)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/lb-2/lb-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### LB-3 - 本番ワークロードにアウトバウンドルールの代わりにNATゲートウェイを使用します

**Category: Availability**

**Impact: Medium**

**Guidance**

Standard Public Load Balancer のアウトバウンド規則では、各バックエンド プール インスタンスに一定量のポートを手動で割り当てる必要があります。SNAT ポートの割り当ては固定されているため、送信規則は送信接続の最もスケーラブルな方法を提供しません。運用環境のワークロードでは、SNAT ポートの枯渇による接続エラーのリスクを回避するために、代わりに NAT Gateway を使用することをお勧めします。NAT Gateway は動的に拡張され、インターネットへの安全な接続を提供します。

**Resources**

- [Resiliency checklist for specific Azure services- Azure Load Balancer](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#azure-load-balancer)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/lb-3/lb-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### LB-4 - Standard Load Balancer がゾーン冗長であることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

Availability Zones があるリージョンでは、Standard Load Balancer にゾーン冗長フロントエンド IP アドレスを割り当てることで、ゾーン冗長にすることができます。ゾーン冗長フロントエンド IP を使用すると、1 つの可用性ゾーンで障害が発生しても、トラフィックを受信できる他の正常なゾーンとそれに対応する正常なバックエンド インスタンスがこれらのゾーンにある限り、ロード バランサーはトラフィックを分散し続けます。

**Resources**

- [Load Balancer and Availability Zones](https://learn.microsoft.com/ja-jp/azure/load-balancer/load-balancer-standard-availability-zones#zone-redundant)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/lb-4/lb-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
