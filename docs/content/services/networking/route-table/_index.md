+++
title = "Route Table"
description = "Best practices and resiliency recommendations for Route Table and associated resources and settings."
date = "8/31/23"
author = "beheath"
msAuthor = "benheath"
draft = false
+++

The presented resiliency recommendations in this guidance include Route Table and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State   | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------: | :-----------------: |
| [RT-1 - Monitor changes in Route Tables with Azure Monitor](#rt-1---monitor-changes-in-route-tables-with-azure-monitor) | Monitoring | Low | Preview  |         Yes         |
| [RT-2 - Configure locks for Route Tables to avoid accidental changes or deletion](#rt-2---configure-locks-for-route-tables-to-avoid-accidental-changes-or-deletion) | Governance         | Low | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### RT-1 - Azure Monitor を使用してルート テーブルの変更を監視します

**Category: Monitoring**

**Impact: Low**

**Guidance**

Azure Monitor を使用したルート テーブルの作成や更新などの管理操作のアラートを作成して、運用リソースに対する承認されていない/望ましくない変更を検出するために、このアラートは、ファイアウォールのバイパスや外部からのリソースへのアクセスの試みなど、ルーティングの望ましくない変更を特定するのに役立ちます。

**Resources**

- [Azure activity log - Azure Monitor | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-monitor/essentials/activity-log?tabs=powershell)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/rt-1/rt-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### RT-2 - 不注意による変更や削除を避けるためにルートテーブルのロックを構成します

**Category: Governance**

**Impact: Low**

**Guidance**

管理者は、Azure サブスクリプション、リソース グループ、またはリソースをロックして、ユーザーが誤って削除したり変更したりしないように保護できます。ロックは、すべてのユーザー権限よりも優先されます。
削除または変更を禁止するロックを設定できます。ポータルでは、これらのロックは 削除 と 読み取り専用 と呼ばれます。

**Resources**

- [Protect your Azure resources with a lock - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/management/lock-resources?toc=%2Fazure%2Fvirtual-network%2Ftoc.json&tabs=json)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/rt-2/rt-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
