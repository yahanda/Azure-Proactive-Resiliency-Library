+++
title = "5 - Monitor"
description = "Microsoft Azure Well-Architected Framework best practices and recommendations for the Reliability Stage - 5 - Monitor"
date = "9/18/23"
weight = 5
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented Microsoft Azure Well-Architected Framework recommendations in this guidance include Reliability Stage "5 - Monitor (Observability and Monitoring)" and associated resources and their settings.

Ongoing monitoring is essential for maintaining system reliability. Key performance indicators (KPIs) are constantly observed to ensure the system is meeting its defined objectives. Services like Azure Monitor, Network Watcher, and Service Health can be invaluable here.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                             |  Category     |  Impact    |  State    | ARG Query Available |
| :----------------------------------------------------------------------------------------------------------------------------------------- | :-----------: | :------:   | :------:  | :-----------------: |
| [WAMN-1 - Make sure your application's health is being monitored](#wamn-1---make-sure-your-applications-health-is-being-monitored)         | Monitoring    | Medium     | Verified  |         No          |
| [WAMN-2 - Define a health model based on performance, availability, and recovery targets](#wamn-2---define-a-health-model-based-on-performance-availability-and-recovery-targets) | Monitoring    | Low        | Verified  |         No          |
| [WAMN-3 - Create Dashboards and Alerts for Azure Platform resources](#wamn-3---create-dashboards-and-alerts-for-azure-platform-resources) | Monitoring    | Low        | Verified  |         No          |
| [WAMN-4 - Ensure that the right people in your organization will be notified about any future service issues](#wamn-4---ensure-that-the-right-people-in-your-organization-will-be-notified-about-any-future-service-issues) | Monitoring    | Medium     | Verified  |         No          |
| [WAMN-5 - Utilize built-in Resilience policies](#wamn-5---utilize-built-in-resilience-policies) | Governance | Medium | Verified | No |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### WAMN-1 - アプリケーションの正常性が監視されていることを確認します

**Category: Monitoring**

**Impact: Medium**

**Recommendation/Guidance**

監視と診断は、可用性と回復性にとって重要です。何かが失敗した場合は、失敗したこと、失敗した時期、および失敗した理由を知る必要があります。

監視は、障害検出と同じではありません。たとえば、アプリケーションが一時的なエラーを検出して再試行し、ダウンタイムを回避できます。また、エラー率を監視してアプリケーションの正常性の全体像を把握できるように、再試行操作もログに記録する必要があります。

キーポイント:

- 実用的で効果的に優先順位付けされたアラートを定義します。
- 制限とクォータに近づいているサービスをポーリングするアラートを作成します。
- アプリケーションインストゥルメンテーションを使用して、パフォーマンスの異常を検出して解決します。
- 実行時間の長いプロセスの進行状況を追跡します。
- 問題のトラブルシューティングを行い、アプリケーションの正常性の全体像を把握します。
- 監視対象の信号を分析、診断、および応答する方法を文書化する

**Resources**

- [Monitoring application health for reliability](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/monitoring)

<br><br>

### WAMN-2 - パフォーマンス、可用性、および回復のターゲットに基づいて正常性モデルを定義します

**Category: Monitoring**

**Impact: Low**

**Recommendation/Guidance**

正常性モデルでは、重要なシステム フローまたは主要なサブシステムの正常性を明らかにして、適切な運用の優先順位付けが適用されるようにする必要があります。たとえば、正常性モデルは、ユーザー サインイン トランザクション フローの現在の状態を表すことができる必要があります。

正常性モデルでは、すべてのエラーを同じように扱うべきではありません。正常性モデルでは、一時的な障害と非一時的な障害を区別する必要があります。予期される一時的だが回復可能な障害と、実際の障害状態を明確に区別する必要があります。

キーポイント:

- アプリケーションが正常か異常かを判断する方法を知っている。
- 診断データ内のログの影響を理解する。
- アプリケーション全体で診断設定を一貫して使用するようにします。
- 正常性モデルで重要なシステム フローを使用する。

**Resources**

- [Health modeling for reliability](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/monitor-model)

<br><br>

### WAMN-3 - Azure Platform リソースのダッシュボードとアラートを作成します

**Category: Monitoring**

**Impact: Low**

**Recommendation/Guidance**

この段階では、オペレーターが問題や傾向にすばやく気付くことができるように、テレメトリデータが表示されます。
例としては、ワークブック、ダッシュボード、メールアラートなどがあります。Azure ブックやダッシュボードを使用すると、Application Insights、Log Analytics、Azure Monitor メトリック、サービス正常性から発生する監視グラフの 1 つのウィンドウを構築できます。Azure Monitor アラートを使用すると、サービスの正常性とリソースの正常性に関するアラートを作成できます。

**Resources**

- [Azure Workbooks templates](https://learn.microsoft.com/ja-jp/azure/azure-monitor/visualize/workbooks-templates)

<br><br>

### WAMN-4 - 今後のサービス問題について、組織内の適切な担当者に通知されるようにします

**Category: Monitoring**

**Impact: Medium**

**Recommendation/Guidance**

Azure には、クラウド リソースの正常性に関する情報を常に把握するための一連のエクスペリエンスが用意されています。Service Health ポータルでは、リソースに影響を与える可能性がある 4 種類の正常性イベントが追跡されます。

- サービスの問題 - 現在影響している Azure サービスの問題 (停止)
- 計画メンテナンス - 将来のサービスの可用性に影響を与える可能性のある今後のメンテナンス。
- 正常性アドバイザリ - 注意が必要な Azure サービスの変更。例としては、Azure 機能の非推奨やアップグレード要件 (サポートされている PHP フレームワークへのアップグレードなど) などがあります。
- セキュリティ アドバイザリ - Azure サービスの可用性に影響を与える可能性のあるセキュリティ関連の通知または違反。

**Resources**

- [Create a Service Health alert using the Azure portal](https://learn.microsoft.com/ja-jp/azure/service-health/alerts-activity-log-service-notifications-portal#create-a-service-health-alert-using-the-azure-portal)

<br><br>

### WAMN-5 - 組み込みのレジリエンスポリシーを利用します

**Category: Governance**

**Impact: Medium**

**Recommendation/Guidance**

Azure の組み込みの回復性ポリシーを利用して、Azure サービスの回復性のある構成を監査および適用します。Azure Policy は、組織の標準を適用し、コンプライアンスを大規模に評価するのに役立ちます。

**Resources**

- [Built-in Resilience policy definitions](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policyDefinitions/Resilience)
- [Get policy compliance data](https://learn.microsoft.com/ja-jp/azure/governance/policy/how-to/get-compliance-data)

<br><br>
