+++
title = "ExpressRoute Connection"
description = "Best practices and resiliency recommendations for ExpressRoute Connection and associated resources and settings."
date = "1/28/24"
author = "ehaslett"
msAuthor = "ethaslet"
draft = false
+++

The presented resiliency recommendations in this guidance include ExpressRoute Connection and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------: | :------: | :-----------------: |
| [ERCON-1 - For Connections using ExpressRoute Direct circuits and UltraPerformance or ErGw3AZ ExpressRoute Gateways, enable FastPath to improve data path performance between your on-premises network and your virtual network](#ercon-1---for-connections-using-expressroute-direct-circuits-and-ultraperformance-or-ergw3az-expressroute-gateways-enable-fastpath-to-improve-data-path-performance-between-your-on-premises-network-and-your-virtual-network) | System Efficiency | Medium | Verified | No |
| [ERCON-2 - Configure an Azure Resource Lock on connections to prevent accidental deletion](#ercon-2---configure-an-azure-resource-lock-on-connections-to-prevent-accidental-deletion) | Availability | High | Verified | No |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ERCON-1 - ExpressRoute Direct 回線と UltraPerformance または ErGw3AZ ExpressRoute ゲートウェイを使用する接続の場合、FastPath を有効にして、オンプレミス ネットワークと仮想ネットワーク間のデータ パスのパフォーマンスを向上させます

**Category: System Efficiency**

**Impact: Medium**

**Recommendation/Guidance**

ExpressRoute 仮想ネットワーク ゲートウェイは、ネットワーク ルートを交換し、ネットワーク トラフィックをルーティングするように設計されています。FastPath は、オンプレミス ネットワークと仮想ネットワーク間のデータ パスのパフォーマンスを向上させるように設計されています。有効にすると、FastPath はゲートウェイをバイパスして、仮想ネットワーク内の仮想マシンにネットワーク トラフィックを直接送信します。ゲートウェイをバイパスすると、ゲートウェイの使用率が減り、回復性が向上します。

**Resources**

- [About ExpressRoute FastPath](https://learn.microsoft.com/ja-jp/azure/expressroute/about-fastpath)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ercon-1/ercon-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ERCON-2 - 誤って削除されないように接続に Azure リソース ロックを構成します

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

ゲートウェイ接続リソースの Azure リソース ロックを構成して、誤って削除されないようにします。ゲートウェイ接続リソースを誤って削除すると、オンプレミス ネットワークと Azure ワークロード間の接続が予期せず失われる可能性があります。管理者は、Azure サブスクリプション、リソース グループ、またはリソースをロックして、ユーザーが誤って削除したり変更したりしないように保護できます。ロックは、すべてのユーザー権限よりも優先されます。

**Resources**

- [Protect your Azure resources with a lock - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/management/lock-resources?tabs=json)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ercon-2/ercon-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
