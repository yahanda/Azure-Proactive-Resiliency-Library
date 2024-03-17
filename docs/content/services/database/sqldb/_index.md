+++
title = "SQL DB"
description = "Best Practices and Resiliency Recommendations for Azure SQL Database"
date = "2023-05-30"
author = "temalo"
msAuthor = "temalo"

draft = false
+++

The presented resiliency recommendations in this guidance include Azure Database Services

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                                                 |        Category        | Impact |  State  | ARG Query Available |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------:|:------:|:-------:|:-------------------:|
| [SQLDB-1 - Use Active Geo Replication to Create a Readable Secondary in Another Region](#sqldb-1---use-active-geo-replication-to-create-a-readable-secondary-in-another-region)                                                                |   Disaster Recovery    |  High  | Preview |         Yes         |
| [SQLDB-2 - Use Auto Failover Groups that can include one or multiple databases, typically used by the same application](#sqldb-2---use-auto-failover-groups-that-can-include-one-or-multiple-databases-typically-used-by-the-same-application) |   Disaster Recovery    |  High  | Preview |         Yes         |
| [SQLDB-3 - Use a Zone-Redundant database](#sqldb-3---use-a-zone-redundant-database)                                                                                                                                                            |      Availability      | Medium | Preview |         Yes         |
| [SQLDB-4 - Implement Retry Logic](#sqldb-4---implement-retry-logic)                                                                                                                                                                            | Application Resilience |  High  | Preview |         Yes         |
| [SQLDB-5 - Monitor your Azure SQL Database in near-real time to detect reliability incidents](#sqldb-5---monitor-your-azure-sql-database-in-near-real-time-to-detect-reliability-incidents)                                                    |       Monitoring       |  High  | Preview |         Yes         |
| [SQLDB-6 - Back up your keys](#sqldb-6---back-up-your-keys)                                                                                                                                                                                    |   Disaster Recovery    | Medium | Preview |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### SQLDB-1 - アクティブ geo レプリケーションを使用して、別のリージョンに読み取り可能なセカンダリを作成します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

プライマリ データベースに障害が発生した場合は、セカンダリ データベースへの手動フェールオーバーを実行します。フェールオーバーするまで、セカンダリ データベースは読み取り専用のままです。アクティブ geo レプリケーションを使用すると、読み取り可能なレプリカを作成し、データセンターの停止やアプリケーションのアップグレードが発生した場合に、任意のレプリカに手動でフェールオーバーできます。同じリージョンまたは異なるリージョンで最大 4 つのセカンダリがサポートされ、セカンダリは読み取り専用アクセス クエリにも使用できます。フェールオーバーは、アプリケーションまたはユーザーが手動で開始する必要があります。フェイルオーバー後、新しいプライマリーの接続エンドポイントは異なります。

**Resources**

- [Active Geo Replication](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/active-geo-replication-overview)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sqldb-1/sqldb-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SQLDB-2 - 1 つまたは複数のデータベース (通常は同じアプリケーションで使用) を含めることができる自動フェールオーバー グループを使用します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

読み取り可能なセカンダリ データベースを使用して、読み取り専用クエリ ワークロードをオフロードできます。自動フェールオーバー グループには複数のデータベースが含まれるため、これらのデータベースはプライマリ サーバー上で構成する必要があります。自動フェールオーバー グループでは、グループ内のすべてのデータベースを、別のリージョン内の 1 つのセカンダリ サーバーまたはインスタンスにのみレプリケーションできます。

**Resources**

- [AutoFailover Groups](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/auto-failover-group-overview?tabs=azure-powershell)
- [DR Design](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/designing-cloud-solutions-for-disaster-recovery)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sqldb-2/sqldb-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SQLDB-3 - ゾーン冗長データベースを使用します

**Category: Availability**

**Impact: Medium**

**Guidance**

既定では、Premium 可用性モデルのノードのクラスターは、同じデータセンターに作成されます。Azure Availability Zones の導入により、SQL Database では、Business Critical データベースの異なるレプリカを同じリージョン内の異なる可用性ゾーンに配置できます。単一障害点を排除するために、制御リングは 3 つのゲートウェイ リング(GW)として複数のゾーンに複製されます。特定のゲートウェイ リングへのルーティングは、Azure Traffic Manager (ATM) によって制御されます。Premium または Business Critical サービス レベルのゾーン冗長構成では、追加のデータベース冗長性は作成されないため、追加コストなしで有効にできます。

**Resources**

-[Zone Redundant Databases](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/high-availability-sla)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sqldb-3/sqldb-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SQLDB-4 - 再試行ロジックを実装します

**Category: Application Resilience**

**Impact: High**

**Guidance**

Azure SQL Database は、推移的なインフラストラクチャの障害に関しては回復性がありますが、これらの障害は接続に影響を与える可能性があります。SQL Database の操作中に一時的なエラーが発生した場合は、コードで呼び出しを再試行できることを確認します。

**Resources**

- [How to Implement Retry Logic](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/troubleshoot-common-connectivity-issues)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sqldb-4/sqldb-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SQLDB-5 - Azure SQL Database をほぼリアルタイムで監視して信頼性インシデントを検出します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

利用可能なソリューションの 1 つを使用して SQL DB を監視し、潜在的な信頼性インシデントを早期に検出し、データベースの信頼性を高めます。ほぼリアルタイムの監視ソリューションを選択して、インシデントに迅速に対応します。

**Resources**

- [Azure Monitor](https://learn.microsoft.com/ja-jp/azure/azure-monitor/insights/azure-sql#analyze-data-and-create-alerts)
- [Azure SQL Database Monitoring](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/monitoring-sql-database-azure-monitor)
- [Monitoring SQL Database Reference](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/monitoring-sql-database-azure-monitor-reference)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sqldb-5/sqldb-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SQLDB-6 - キーをバックアップします

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

Azure Key Vault (AKV) を使用して、Always Encrypted 構成に関連する暗号化キーを格納することを強くお勧めしますが、必須ではありません。AKV を使用していない場合は、キーが適切にバックアップされていることを確認します。

**Resources**

- [Azure Key Vault](https://learn.microsoft.com/ja-jp/azure/key-vault/general/overview)
- [Getting Started with Always Encrypted](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/always-encrypted-landing?view=azuresql)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sqldb-6/sqldb-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
