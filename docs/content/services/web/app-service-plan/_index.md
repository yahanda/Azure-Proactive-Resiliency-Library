+++
title = "App Service Plan"
description = "Best practices and resiliency recommendations for App Service Plan and associated resources."
date = "6/27/23"
author = "kunalbabre"
msAuthor = "kunalbabre"
draft = false
+++

The presented resiliency recommendations in this guidance include App Service Plan and associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                         |     Category      | Impact |  State  | ARG Query Available |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [ASP-1 - Migrate App Service to availability Zone Support](#asp-1---migrate-app-service-to-availability-zone-support)                                                                                                  |   Availability    |  High  | Preview |         Yes         |
| [ASP-2 - Use Standard or Premium tier](#asp-2---use-standard-or-premium-tier)                                                                                                                                          |   Availability    |  High  | Preview |         Yes         |
| [ASP-3 - Avoid scaling up or down](#asp-3---avoid-scaling-up-or-down)                                                                                                                                                  | System Efficiency | Medium | Preview |         Yes         |
| [ASP-4 - Create separate App Service plans for production and test](#asp-4---create-separate-app-service-plans-for-production-and-test)                                                                                |    Governance     |  High  | Preview |         No          |
| [ASP-5 - Enable Autoscale/Automatic scaling to ensure adequate resources are available to service requests](#asp-5---enable-autoscaleautomatic-scaling-to-ensure-adequate-resources-are-available-to-service-requests) | System Efficiency | Medium | Preview |         Yes         |
{{< /table >}}
{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ASP-1 - App Service を可用性ゾーン サポートに移行します

**Category: Availability**

**Impact: High**

**Guidance**

可用性ゾーン (AZ) 間での App Service プランと App Service Environment のデプロイは、ビジネスクリティカルなワークロードの回復性と信頼性を強化するために Azure によって提供される機能です。アプリケーションを複数のアベイラビリティーゾーンに分散することで、データセンターレベルの障害が発生した場合でも、アプリケーションの運用を継続できます。このアプローチでは、アプリケーションを異なる Azure リージョンにデプロイする必要なく、優れた冗長性が提供されます。可用性ゾーンは、より高いレベルのフォールトトレランスを提供し、アプリケーションを保護し、ダウンタイムを最小限に抑えるのに役立ちます。これにより、ビジネスの継続性を維持し、中断のないサービスをお客様に提供できます。

**Resources**

- [Migrate App Service to availability zone support](https://learn.microsoft.com/ja-jp/azure/reliability/migrate-app-service)
- [High availability enterprise deployment using App Service Environment](https://learn.microsoft.com/ja-jp/azure/architecture/reference-architectures/enterprise-integration/ase-high-availability-deployment)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/asp-1/asp-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ASP-2 - Standard または Premium レベルを使用します

**Category: Availability**

**Impact: High**

**Guidance**

Azure App Service プランの Standard または Premium レベルの使用は、高度なスケーリング、高可用性、トラフィック管理、強化されたパフォーマンス、ネットワーク機能、複数のデプロイ スロットを提供し、潜在的な障害や需要の増加に直面しても中断のない運用と堅牢性を保証するため、回復性の高いアプリケーションにとって非常に重要です。

**Resources**

- [Resiliency checklist for specific Azure services](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#app-service)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/asp-2/asp-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ASP-3 - スケールアップまたはスケールダウンを避けるようにします

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

Azure App Service インスタンスを頻繁にスケールアップまたはスケールダウンすることは避けることをお勧めします。代わりに、一般的なワークロードを処理できる適切なレベルとインスタンスサイズを選択し、トラフィック量の変化に対応するためにインスタンスをスケールアウトします。スケールアップまたはスケールダウンすると、アプリケーションの再起動がトリガーされ、サービスが中断される可能性があります。

**Resources**

- [Resiliency checklist for specific Azure services](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#app-service)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/asp-3/asp-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ASP-4 - 運用環境とテスト用に個別の App Service プランを作成します

**Category: Governance**

**Impact: High**

**Guidance**

運用環境とテスト環境用に別々の App Service プランを作成することを強くお勧めします。テスト目的で運用環境内のスロットを使用することは避けてください。同じ App Service プラン内のアプリが VM インスタンスを共有する場合、運用環境とテスト デプロイを組み合わせると、運用環境に悪影響を及ぼす可能性があります。たとえば、テスト展開で実行されるロード テストでは、実際の運用サイトのパフォーマンスが低下する可能性があります。テスト展開を個別の計画に分離することで、運用バージョンの分離と保護を確実に行うことができます。

**Resources**

- [Resiliency checklist for specific Azure services](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#app-service)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/asp-4/asp-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ASP-5 - 自動スケーリング/自動スケーリングを有効にして、サービス要求に十分なリソースを使用できるようにします

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

Azure App Service の自動スケーリング/自動スケーリングを有効にして、受信要求を処理するのに十分なリソースを確保することを強くお勧めします。自動スケーリングはルールベースのスケーリングですが、自動スケーリングは、HTTP トラフィックに基づいて自動スケールアウトとスケールインを実行する新しいプラットフォーム機能です。

**Resources**

- [Automatic scaling in Azure App Service](https://learn.microsoft.com/ja-jp/azure/app-service/manage-automatic-scaling?tabs=azure-portal)
- [Auto Scale Web Apps](https://learn.microsoft.com/ja-jp/azure/azure-monitor/autoscale/autoscale-get-started)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/asp-5/asp-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
