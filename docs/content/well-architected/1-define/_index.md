+++
title = "1 - Define"
description = "Microsoft Azure Well-Architected Framework best practices and recommendations for the Reliability Stage - 1 - Define"
date = "9/18/23"
weight = 1
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented Microsoft Azure Well-Architected Framework recommendations in this guidance include Reliability Stage "1 - Define (Requirements)" and associated resources and their settings.

In this initial stage, the objectives and requirements for system reliability are established. This often involves specifying availability and recovery targets, latency tolerances, criticality classifications, and disaster recovery objectives.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                                      |  Category           |  Impact         |  State            | ARG Query Available |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-----------------: | :------:        | :------:          | :-----------------: |
| [WADF-1 - Ensure the Availability Targets are well defined and communicated across teams working on the Workload](#wadf-1---ensure-the-availability-targets-are-well-defined-and-communicated-across-teams-working-on-the-workload) | Availability        | High            | Verified          |         No          |
| [WADF-2 - Ensure the Recovery Targets are well defined and communicated across teams working on the Workload](#wadf-2---ensure-the-recovery-targets-are-well-defined-and-communicated-across-teams-working-on-the-workload)         | Disaster Recovery   | High            | Verified          |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### WADF-1 - 可用性ターゲットが明確に定義され、ワークロードに取り組んでいるチーム間で伝達されていることを確認します

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

可用性目標 (SLA、SLO、SLI) が明確に定義され、テストされ、監視され、ワークロードに取り組んでいるチーム間で伝達されていることを確認します。

サービス レベル アグリーメント (SLA) は、アプリケーションのパフォーマンスと可用性に関するコミットメントを表す可用性目標です。システム内の個々のコンポーネントのSLAを理解することは、信頼性目標を定義するために不可欠です。依存関係の SLA を知ることは、依存関係を高可用性にし、適切なサポート契約を結ぶときに、追加の支出を正当化する理由にもなります。アプリケーションによって利用される依存関係の可用性目標を理解し、理想的にはアプリケーション ターゲットと整合させることも考慮する必要があります。

可用性の期待を理解することは、アプリケーションの全体的な操作を確認するために不可欠です。

たとえば、99.999% のアプリケーションのサービス レベル目標 (SLO) の達成を目指している場合、アプリケーションに必要な固有の運用アクションのレベルは、99.9% の SLO が目標である場合よりもはるかに高くなります。

**Resources**

- [Use business metrics to design resilient Azure applications](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/business-metrics#workload-availability-targets)
- [Target functional and nonfunctional requirements](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/design-requirements)

<br><br>

### WADF-2 - リカバリターゲットが明確に定義され、ワークロードに取り組んでいるチーム間で伝達されていることを確認します

**Category: Disaster Recovery**

**Impact: High**

**Recommendation/Guidance**

復旧目標が明確に定義され、ワークロードに取り組んでいるチーム間で伝達されていることを確認します。
考慮すべき 2 つの重要なメトリックは、ディザスター リカバリーに関連する目標復旧時間と目標復旧ポイントです。

- 目標復旧時間 (RTO) は、インシデント発生後にアプリケーションを使用できない最大許容時間です。RTO が 90 分の場合、障害発生から 90 分以内にアプリケーションを実行状態に復元できる必要があります。RTO が非常に低い場合は、リージョンの停止から保護するために、スタンバイでアクティブ/パッシブ構成を継続的に実行している 2 番目のリージョン デプロイを維持できます。場合によっては、アクティブ/アクティブ構成を展開して、RTO をさらに低く抑えることができます。
- 目標復旧時点 (RPO) は、障害発生時に許容されるデータ損失の最大期間です。たとえば、データを 1 つのデータベースに格納し、他のデータベースへのレプリケーションを行わず、1 時間ごとにバックアップを実行すると、最大 1 時間分のデータが失われる可能性があります。
RTO と RPO はシステムの非機能要件であり、ビジネス要件によって決定される必要があります。これらの値を導き出すには、リスク評価を実施し、ダウンタイムやデータ損失のコストを明確に理解することをお勧めします。

アプリケーションの可用性を監視および測定することは、アプリケーション全体の正常性と、定義された目標に対する進捗状況を評価するために不可欠です。次のような主要なターゲットを必ず測定および監視してください。

- 平均故障間隔(MTBF) — 特定のコンポーネントの故障間の平均時間。
- 平均復旧時間(MTTR) — 障害発生後のコンポーネントの復元にかかる平均時間。

**Resources**

- [Target functional and nonfunctional requirements](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/design-requirements)

<br><br>
