+++
title = "Cosmos DB"
description = "Best practices and resiliency recommendations for Cosmos DB and associated resources and settings."
date = "6/30/23"
author = "kovarikthomas"
msAuthor = "tokovari"
draft = false
+++

The presented resiliency recommendations in this guidance include Cosmos DB and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                  |        Category        | Impact |  State  | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------:|:------:|:-------:|:-------------------:|
| [COSMOS-1 – Configure at least two regions for high availability](#cosmos-1---configure-at-least-two-regions-for-high-availability)                                                             |      Availability      |  High  | Preview |         Yes         |
| [COSMOS-2 – Enable service-managed failover for multi-region accounts with single write region](#cosmos-2---enable-service-managed-failover-for-multi-region-accounts-with-single-write-region) |   Disaster Recovery    |  High  | Preview |         No          |
| [COSMOS-3 – Evaluate multi-region write capability](#cosmos-3---evaluate-multi-region-write-capability)                                                                                         |   Disaster Recovery    |  High  | Preview |         Yes         |
| [COSMOS-4 – Choose appropriate consistency mode reflecting data durability requirements](#cosmos-4---choose-appropriate-consistency-mode-reflecting-data-durability-requirements)               |   Disaster Recovery    |  High  | Preview |         No          |
| [COSMOS-5 – Configure continuous backup mode](#cosmos-5---configure-continuous-backup-mode)                                                                                                     |   Disaster Recovery    |  High  | Preview |         Yes         |
| [COSMOS-6 – Ensure query results are fully drained](#cosmos-6---ensure-query-results-are-fully-drained)                                                                                         |   System Efficiency    |  High  | Preview |         No          |
| [COSMOS-7 – Maintain singleton pattern in your client](#cosmos-7---maintain-singleton-pattern-in-your-client)                                                                                   |   System Efficiency    | Medium | Preview |         No          |
| [COSMOS-8 – Implement retry logic in your client](#cosmos-8---implement-retry-logic-in-your-client)                                                                                             | Application Resilience | Medium | Preview |         No          |
| [COSMOS-9 – Monitor Cosmos DB health and set up alerts](#cosmos-9---monitor-cosmos-db-health-and-set-up-alerts)                                                                                 |       Monitoring       | Medium | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### COSMOS-1 - 高可用性のために少なくとも 2 つのリージョンを構成します

**Category: Availability**

**Impact: High**

**Guidance**

Azure では、ラック、DC、ゾーン、リージョンの分離レベルによる多層分離アプローチが実装されています。Cosmos DB は、既定では 4 つのレプリカを実行することで高い回復性を備えていますが、リージョン全体または可用性ゾーンに関する障害や問題の影響を受けやすいままです。そのため、より高い SLA を実現するには、Cosmos DB で少なくともセカンダリ リージョンを有効にすることが重要です。そうすることでダウンタイムはまったく発生せず、地図上のピンを選択するのと同じくらい簡単です。

**Resources**

- [Distribute data globally with Azure Cosmos DB | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/distribute-data-globally)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-1/cosmos-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-2 - 単一の書き込みリージョンを持つマルチリージョン アカウントのサービスマネージド フェールオーバーを有効にします

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Cosmos DB は、非常に高いアップタイムと回復性を備えた実証済みのサービスですが、最も回復性のあるシステムでも、小さな問題が発生することがあります。リージョンが使用できなくなった場合、サービス マネージド フェールオーバー オプションを使用すると、Azure Cosmos DB は、ユーザーの操作を必要とせずに、次に使用可能なリージョンに自動的にフェールオーバーできます。

**Resources**

- [Manage an Azure Cosmos DB account by using the Azure portal | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/how-to-manage-database-account#automatic-failover)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-2/cosmos-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-3 - 複数リージョンの書き込み機能を評価します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

複数リージョンの書き込み機能を使用すると、複数のリージョンでアクティブであるため、本質的に可用性の高いマルチリージョン アプリケーションを設計できます。ただし、そのためには、整合性の要件を綿密に検討し、競合解決ポリシーを使用して潜在的な書き込み競合を処理する必要があります。反対に、この構成を盲目的に有効にすると、予期しないアプリケーションの動作や、未処理の競合によるデータの破損により、可用性が低下する可能性があります。

**Resources**

- [Distribute data globally with Azure Cosmos DB | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/distribute-data-globally)
- [Conflict resolution types and resolution policies in Azure Cosmos DB | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/conflict-resolution-policies)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-3/cosmos-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-4 - データの持続性要件を反映した適切な整合性モードを選択します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

グローバルに分散されたデータベース環境内では、整合性レベルと、リージョン全体の停止が発生した場合のデータ持続性の間には直接的な関係があります。事業継続計画を策定する際には、破壊的なイベント後の復旧時にアプリケーションが損失を許容できる最近のデータ更新の最大期間を理解する必要があります。より強力な整合性モードが必要であることが確立され、書き込みレイテンシーの増加を許容し、読み取り専用リージョンでの停止が書き込みを受け入れる書き込みリージョンの機能に影響を与える可能性があることを理解していない限り、セッション整合性を使用することをお勧めします。

**Resources**

- [Consistency level choices - Azure Cosmos DB | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/consistency-levels)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-4/cosmos-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-5 - 継続的バックアップ モードを構成します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Cosmos DB によってデータが自動的にバックアップされるため、バックアップをオフにする方法はありません。要するに、あなたは常に保護されています。しかし、プロセスがうまくいかず、削除すべきでないデータを削除したり、顧客データが誤って上書きされたりなどの事故が発生した場合は、変更を元に戻すのにかかる時間を最小限に抑えることが重要です。継続モードでは、このような事故が発生する前の時点にデータベース/コレクションをセルフサービスで復元できます。ただし、定期モードでは、Microsoft サポートに連絡する必要があり、迅速なサポートを提供するよう努めているにもかかわらず、復元時間が必然的に長くなります。

**Resources**

- [Continuous backup with point in time restore feature in Azure Cosmos DB | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/continuous-backup-restore-introduction)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-5/cosmos-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-6 - クエリ結果が完全にドレインされるようにします

**Category: System Efficiency**

**Impact: High**

**Guidance**

Cosmos DB では、1 つの応答が 4 MB に制限されています。クエリが大量のデータまたは複数のバックエンド パーティションからのデータを要求する場合、結果は複数のページにまたがり、個別の要求を発行する必要があります。各結果ページには、さらに結果が利用可能かどうかが示され、次のページにアクセスするための継続トークンが提供されます。コードに while ループを含め、結果が得られなくなるまでページを走査する必要があります。

**Resources**

- [Pagination in Azure Cosmos DB | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/nosql/query/pagination#handling-multiple-pages-of-results)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-6/cosmos-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-7 - クライアントでシングルトン パターンを維持します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

新しいデータベース接続の確立にはコストがかかるだけでなく、その維持にもコストがかかります。そのため、SDK クライアントのインスタンス (いわゆる "シングルトン") を 1 つのアカウントごとに、アプリケーションごとに 1 つだけ維持することが重要です。接続 (HTTP と TCP) はどちらも、クライアント インスタンスにスコープが設定されます。ほとんどのコンピューティング環境には、同時に開くことができる接続の数に関する制限があり、これらの制限に達すると、接続が影響を受けます。

**Resources**

- [Designing resilient applications with Azure Cosmos DB SDKs | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/nosql/conceptual-resilient-sdk-applications)
**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-7/cosmos-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-8 - クライアントに再試行ロジックを実装します

**Category: Application Resilience**

**Impact: Medium**

**Guidance**

既定では、Cosmos DB SDK は多数の一時的なエラーを処理し、可能な場合は自動的に操作を再試行します。ただし、アプリケーションでは、SDK で一般的に処理できない明確に定義された特定のケースに対して再試行ポリシーを追加する必要があります。

**Resources**

- [Designing resilient applications with Azure Cosmos DB SDKs | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/nosql/conceptual-resilient-sdk-applications)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-8/cosmos-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### COSMOS-9 - Cosmos DB の正常性を監視し、アラートを設定します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

Azure Cosmos DB リソースの可用性と応答性を監視し、予期しないイベントが発生した場合にワークロードをプロアクティブに維持できるようにアラートを設定することをお勧めします。

**Resources**

- [Create alerts for Azure Cosmos DB using Azure Monitor | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/cosmos-db/create-alerts)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cosmos-9/cosmos-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
