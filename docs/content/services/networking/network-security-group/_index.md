+++
title = "Network Security Group"
description = "Best practices and resiliency recommendations for Network Security Group and associated resources and settings."
date = "9/19/23"
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented resiliency recommendations in this guidance include Network Security Group and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                |  Category         |  Impact   |  State     | ARG Query Available |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------:  | :------:  | :------:   | :-----------------: |
| [NSG-1 - Configure Diagnostic Settings for all network security groups](#nsg-1---configure-diagnostic-settings-for-all-network-security-groups)                                               | Monitoring        |  Medium   | Preview    |     No          |
| [NSG-2 - Monitor changes in Network Security Groups with Azure Monitor](#nsg-2---monitor-changes-in-network-security-groups-with-azure-monitor)                               | Monitoring        |     Low   | Preview    |     Yes          |
| [NSG-3 - Configure locks for Network Security Groups to avoid accidental changes and/or deletion](#nsg-3---configure-locks-for-network-security-groups-to-avoid-accidental-changes-andor-deletion)      | Governance        |     Low   | Preview    |     No          |
| [NSG-4 - Configure NSG Flow Logs](#nsg-4---configure-nsg-flow-logs)                                                                     | Monitoring        |  Medium   | Preview    |     Yes         |
| [NSG-5 - The NSG only has Default Security Rules, make sure to configure the necessary rules](#nsg-5---the-nsg-only-has-default-security-rules-make-sure-to-configure-the-necessary-rules)          | Access & Security |  Medium   | Preview    |     Yes          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### NSG-1 - すべてのネットワーク セキュリティ グループの診断設定を構成します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

リソース ログは、診断設定を作成して 1 つ以上の場所にルーティングするまで収集および保存されません。

**Resources**

- [Diagnostic settings in Azure Monitor](https://learn.microsoft.com/ja-jp/azure/azure-monitor/essentials/diagnostic-settings)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/nsg-1/nsg-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### NSG-2 - Azure Monitor を使用してネットワーク セキュリティ グループの変更を監視します

**Category: Monitoring**

**Impact: Low**

**Guidance**

Azure Monitor を使用したネットワーク セキュリティ グループ規則の作成または更新などの管理操作のアラートを作成する 運用リソースに対する承認されていない/望ましくない変更を検出するために、このアラートは、ファイアウォールのバイパスや外部からのリソースへのアクセスの試みなど、既定のセキュリティの望ましくない変更を特定するのに役立ちます。

**Resources**

- [Azure Monitor activity log](https://learn.microsoft.com/ja-jp/azure/azure-monitor/essentials/activity-log?tabs=powershell)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/nsg-2/nsg-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### NSG-3 - 不注意による変更や削除を回避するためにネットワーク セキュリティ グループのロックを構成します

**Category: Governance**

**Impact: Low**

**Guidance**

管理者は、Azure サブスクリプション、リソース グループ、またはリソースをロックして、ユーザーが誤って削除したり変更したりしないように保護できます。ロックは、すべてのユーザー権限よりも優先されます。
削除または変更を禁止するロックを設定できます。ポータルでは、これらのロックは 削除 と 読み取り専用 と呼ばれます。

**Resources**

- [Lock your resources to protect your infrastructure](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/management/lock-resources?toc=%2Fazure%2Fvirtual-network%2Ftoc.json&tabs=json)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/nsg-3/nsg-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### NSG-4 - NSG フロー ログを構成します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

自社のネットワークを監視、管理、把握し、保護および最適化できるようにすることが重要です。ネットワークの現在の状態、誰が接続しているか、ユーザーがどこから接続しているかを知る必要があります。また、どのポートがインターネットに開かれているか、どのようなネットワーク動作が予想されるか、どのネットワーク動作が不規則であるか、トラフィックの急激な増加がいつ発生するかを知る必要があります。

フローログは、クラウド環境におけるすべてのネットワークアクティビティの信頼できる情報源です。リソースを最適化しようとしているスタートアップ企業でも、侵入を検知しようとしている大企業でも、フローログが役立ちます。これらは、ネットワークフローの最適化、スループットの監視、コンプライアンスの検証、侵入の検出などに使用できます。

**Resources**

- [Flow logging for network security groups](https://learn.microsoft.com/ja-jp/azure/network-watcher/network-watcher-nsg-flow-logging-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/nsg-4/nsg-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### NSG-5 - NSG には既定のセキュリティ規則のみがあり、必要な規則を構成してください

**Category: Access & Security**

**Impact: Medium**

**Guidance**

Azure ネットワーク セキュリティ グループを使用して、Azure 仮想ネットワーク内の Azure リソース間のネットワーク トラフィックをフィルター処理できます。ネットワーク セキュリティ グループには、複数の種類の Azure リソースへの受信ネットワーク トラフィック、または送信ネットワーク トラフィックを許可または拒否するセキュリティ規則が含まれています。ルールごとに、送信元と宛先、ポート、およびプロトコルを指定できます。

**Resources**

- [Security rules](https://learn.microsoft.com/ja-jp/azure/virtual-network/network-security-groups-overview#security-rules)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/nsg-5/nsg-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
