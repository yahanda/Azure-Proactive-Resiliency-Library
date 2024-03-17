+++
title = "Event Grid"
description = "Best practices and resiliency recommendations for Event Grid and associated resources and settings."
date = "8/30/23"
author = "beheath"
msAuthor = "benheath"
draft = false
+++

The presented resiliency recommendations in this guidance include Event Grid and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State   | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------: | :-----------------: |
| [EVG-1 - Configure Diagnostic Settings for all Azure Event Grid resources](#evg-1---configure-diagnostic-settings-for-all-azure-event-grid-resources) | Monitoring | Low | Preview  |         No        |
| [EVG-2 - Configure Dead-letter to save events that cannot be delivered](#evg-2---configure-dead-letter-to-save-events-that-cannot-be-delivered) | Automation          | Low | Preview |         No          |
| [EVG-3 - Configure Private Endpoints](#evg-3---configure-private-endpoints) | Access & Security          | Low | Preview |         Yes          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### EVG-1 - すべての Azure Event Grid リソースの診断設定を構成します

**Category: Monitoring**

**Impact: Low**

**Guidance**

診断設定を有効にすると、診断情報をキャプチャして表示し、障害のトラブルシューティングを行うことができます。次の表は、さまざまな種類の Event Grid リソース (カスタム トピック、システム トピック、ドメイン) で使用できる設定を示しています。

**Resources**

- [Azure Event Grid - Enable diagnostic logs for Event Grid resources](https://learn.microsoft.com/ja-jp/azure/event-grid/enable-diagnostic-logs-topic)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="powershell" file="code/evg-1/evg-1.ps1" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### EVG-2 - 配信できないイベントを保存するための配信不能を構成します

**Category: Automation**

**Impact: Low**

**Guidance**

Event Grid は、特定の期間内にイベントを配信できない場合、またはイベントの配信を一定回数試行した後に、配信されていないイベントをストレージ アカウントに送信できます。このプロセスは、配信不能と呼ばれます。既定では、Event Grid は配信不能を有効にしません。有効にするには、イベント サブスクリプションの作成時に、配信されないイベントを保持するストレージ アカウントを指定する必要があります。このストレージ アカウントからイベントをプルして、配信を解決します。

**Resources**

- [Azure Event Grid delivery and retry](https://learn.microsoft.com/ja-jp/azure/event-grid/delivery-and-retry#dead-letter-events)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="powershell" file="code/evg-2/evg-2.ps1" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### EVG-3 - プライベートエンドポイントを設定します

**Category: Access & Security**

**Impact: Low**

**Guidance**

プライベート エンドポイントを使用すると、パブリック インターネットを経由せずに、プライベート リンク経由で仮想ネットワークからカスタム トピックとドメインにイベントを直接安全にイングレスできます。プライベート エンドポイントでは、カスタム トピックまたはドメインの VNet アドレス空間の IP アドレスが使用されます。

**Resources**

- [Configure private endpoints for Azure Event Grid topics or domains](https://learn.microsoft.com/ja-jp/azure/event-grid/configure-private-endpoints)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/evg-3/evg-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
