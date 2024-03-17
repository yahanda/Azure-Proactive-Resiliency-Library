+++
title = "Log Analytics"
description = "Best practices and resiliency recommendations for Log Analytics and associated resources."
date = "5/9/23"
author = "judyer28"
msAuthor = "judyer"
draft = false
+++

The presented resiliency recommendations in this guidance include Log Analytics and associated Log Analytics settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for Log Analytics and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                              |     Category      | Impact |  State  | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [LOG-1 - Enable Log Analytics data export to GRS or GZRS](#log-1---enable-log-analytics-data-export-to-grs-or-gzrs)                                                                                         | Disaster Recovery | Medium | Preview |         No          |
| [LOG-2 - Link Log Analytics Workspace to an Availability Zone enabled dedicated cluster](#log-2---link-log-analytics-workspace-to-an-availability-zone-enabled-dedicated-cluster)                           |   Availability    | Medium | Preview |         No          |
| [LOG-3 - Configure data collection to send critical data to multiple workspaces in different regions](#log-3---configure-data-collection-to-send-critical-data-to-multiple-workspaces-in-different-regions) | Disaster Recovery | Medium | Preview |         No          |
| [LOG-4 - Create a health status alert rule for your Log Analytics workspace](#log-4---create-a-health-status-alert-rule-for-your-log-analytics-workspace)                                                   |    Monitoring     |  Low   | Preview |         No          |
| [LOG-5 - Configure minimal logging and retention of logs](#log-5---configure-minimal-logging-and-retention-of-logs)                                                                                         |    Monitoring     |  Low   | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### LOG-1 - GRS または GZRS への Log Analytics データのエクスポートを有効にします

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

Log Analytics ワークスペースでデータをエクスポートすると、Azure Storage アカウントにデータを継続的にエクスポートできます。 geo 冗長ストレージ (GRS) または geo ゾーン冗長ストレージ (GZRS) アカウントに継続的にエクスポートすることで、リージョン障害の万が一の事態から Log Analytics ワークスペース データを保護します。

**Resources**

- [Log Analytics workspace data export in Azure Monitor](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/logs-data-export)
- [Azure Monitor configuration recommendations](https://learn.microsoft.com/ja-jp/azure/azure-monitor/best-practices-logs#configuration-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/log-1/log-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### LOG-2 - Log Analytics ワークスペースを可用性ゾーン対応の専用クラスターにリンクします

**Category: Availability**

**Impact: Medium**

**Guidance**

Log Analytics ワークスペースを可用性ゾーン対応の専用クラスターにリンクして、Log Analytics ワークスペースに依存する Azure Monitor 機能の回復性を高め、データセンターの障害という万が一のイベントから Log Analytics データを保護します。

**Resources**

- [Enhance data and service resilience in Azure Monitor Logs with availability zones](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/availability-zones)
- [Create and manage a dedicated cluster in Azure Monitor Logs](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/logs-dedicated-clusters)
- [Azure Monitor configuration recommendations](https://learn.microsoft.com/ja-jp/azure/azure-monitor/best-practices-logs#configuration-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/log-2/log-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### LOG-3 - 重要なデータを異なるリージョンの複数のワークスペースに送信するようにデータ収集を構成します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

リージョン障害の万が一のシナリオでワークスペースを使用可能にする必要がある場合は、異なるリージョンの複数のワークスペースに重要なデータを送信するようにデータ収集を構成します。

**Resources**

- [Azure Monitor configuration recommendations](https://learn.microsoft.com/ja-jp/azure/azure-monitor/best-practices-logs#configuration-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/log-3/log-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### LOG-4 - Log Analytics ワークスペースの正常性状態アラート ルールを作成します

**Category: Monitoring**

**Impact: Low**

**Guidance**

正常性状態アラートは、データセンターまたはリージョンの障害が原因でワークスペースが使用できなくなった場合に事前に通知します。

**Resources**

- [Monitor Log Analytics workspace health](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/log-analytics-workspace-health)
- [Azure Monitor configuration recommendations](https://learn.microsoft.com/ja-jp/azure/azure-monitor/best-practices-logs#configuration-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/log-4/log-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### LOG-5 - ログの最小ログ記録と保持を構成します

**Category: Monitoring**

**Impact: Low**

**Guidance**

Azure Monitor ログは、データの種類に応じて特定の期間 (たとえば、プラットフォーム ログとメトリックの場合は 31 日間) ログ データを自動的に保持します。ただし、コンプライアンスやビジネス上の理由から、データを長期間保持する必要がある場合があります。要件に基づいてデータ保持設定を構成できます。

長期ストレージの場合は、Azure Monitor から Azure Blob Storage などのよりコスト効率の高いストレージ ソリューションにログを移動することが必要になる場合があります。これにより、高いコストをかけずにログを長期間保持できます。

**Resources**

- [Data retention and archive in Azure Monitor Logs](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/data-retention-archive?tabs=portal-1%2Cportal-2)
- [Run search jobs in Azure Monitor](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/search-jobs?tabs=portal-1%2Cportal-2)
- [Restore logs in Azure Monitor](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/restore?tabs=api-1)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/log-5/log-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
