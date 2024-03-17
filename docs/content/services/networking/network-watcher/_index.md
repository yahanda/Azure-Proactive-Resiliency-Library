+++
title = "Network Watcher"
description = "Best practices and resiliency recommendations for Network Watcher and associated resources and settings."
date = "9/19/23"
author = "rodrigosantosms"
msAuthor = "rodrigosantosmsS"
draft = false
+++

The presented resiliency recommendations in this guidance include Network Watcher and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                      |  Category      |  Impact   |  State      | ARG Query Available |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :------------: | :------:  | :------:    | :-----------------: |
| [NW-1 - Deploy Network Watcher in all regions where you have networking services](#nw-1---deploy-network-watcher-in-all-regions-where-you-have-networking-services) | Monitoring     |  Low      | Preview     |         Yes         |
| [NW-2 - Fix Flow Log configurations in Failed state or Disabled Status](#nw-2---fix-flow-log-configurations-in-failed-state-or-disabled-status)                     | Monitoring     |  Low      | Preview     |         Yes          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### NW-1 - ネットワーク サービスがあるすべてのリージョンに Network Watcher をデプロイします

**Category: Monitoring**

**Impact: Low**

**Guidance**

Azure Network Watcher には、Azure IaaS (サービスとしてのインフラストラクチャ) リソースの監視、診断、メトリックの表示、ログの有効化または無効化を行うための一連のツールが用意されています。Network Watcher を使用すると、仮想マシン (VM)、仮想ネットワーク (VNet)、アプリケーション ゲートウェイ、ロード バランサーなどの IaaS 製品のネットワーク正常性を監視および修復できます。 Network Watcher は、PaaS 監視や Web 分析用に設計または意図されていません。

**Resources**

- [What is Azure Network Watcher?](https://learn.microsoft.com/ja-jp/azure/network-watcher/network-watcher-overview)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/nw-1/nw-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### NW-2 - 失敗状態または無効状態のフローログ構成を修正します

**Category: Monitoring**

**Impact: Low**

**Guidance**

ネットワーク セキュリティ グループ フロー ログは、ネットワーク セキュリティ グループを通過する IP トラフィックに関する情報をログに記録できる Azure Network Watcher の機能です。フロー ログが [失敗] 状態の場合、関連付けられたリソースからの監視データは収集されていません。

**Resources**

- [Manage NSG flow logs using the Azure portal](https://learn.microsoft.com/ja-jp/azure/network-watcher/nsg-flow-logging)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/nw-2/nw-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
