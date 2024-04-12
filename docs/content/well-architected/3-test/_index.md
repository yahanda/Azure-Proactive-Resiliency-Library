+++
title = "3 - Test"
description = "Microsoft Azure Well-Architected Framework best practices and recommendations for the Reliability Stage - 3 - Test"
date = "9/18/23"
weight = 3
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented Microsoft Azure Well-Architected Framework recommendations in this guidance include Reliability Stage "3 - Test (Workload Testing)" and associated resources and their settings.

Before deploying the system, comprehensive tests are conducted to validate the design and implementation. This stage is crucial for identifying any weaknesses that could compromise reliability.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                  |  Category              |  Impact  |  State     | ARG Query Available |
| :---------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------: | :------: | :------:   | :-----------------: |
| [WATS-1 - Test your applications for availability and resiliency](#wats-1---test-your-applications-for-availability-and-resiliency)             | Application Resilience | High   |  Verified  |         No          |
| [WATS-2 - Consider building logic into your workload to handle errors](#wats-2---consider-building-logic-into-your-workload-to-handle-errors)   | Application Resilience | High     |  Verified  |         No          |
| [WATS-3 - Perform disaster recovery tests regularly](#wats-3---perform-disaster-recovery-tests-regularly)                                         | Disaster Recovery      | High   |  Verified  |         No          |
| [WATS-4 - Use chaos engineering to test Azure applications](#wats-4---use-chaos-engineering-to-test-azure-applications)                         | Application Resilience | Medium   |  Verified  |         No          |
| [WATS-5 - Test application fault resiliency](#wats-5---test-application-fault-resiliency)                         | Application Resilience | High   |  Verified  |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### WATS-1 - アプリケーションの可用性と回復性をテストします

**Category: Application Resilience**

**Impact: High**

**Recommendation/Guidance**

アプリケーションは、可用性と回復性を確認するためにテストする必要があります。可用性とは、アプリケーションが大幅なダウンタイムなしで正常な状態で実行されている時間を表します。回復性は、アプリケーションが障害から回復する速度を表します。

可用性と回復性を測定できれば、次のような質問に答えることができます。 許容されるダウンタイムはどれくらいですか?潜在的なダウンタイムは、ビジネスにどれくらいのコストをかけますか?可用性の要件は何ですか?アプリケーションの可用性を高めるために、どのくらいの投資をしていますか?リスクとコストは?テストは、アプリケーションがこれらの要件を満たしていることを確認する上で重要な役割を果たします。

キーポイント:

- 定期的にテストして、既存のしきい値、ターゲット、前提条件を検証します。
- テストを可能な限り自動化する。
- 主要なテスト環境と本番環境の両方でテストを実行します。
- 断続的な障害状態でのエンドツーエンドのワークロードのパフォーマンスを確認します。
- パフォーマンスに関する重要な機能要件と非機能要件に照らしてアプリケーションをテストします。
- 予想されるピークボリュームで負荷テストを実施して、負荷がかかった状態でのスケーラビリティとパフォーマンスをテストします。
- フォールトを挿入してカオステストを実行する。

**Resources**

- [Testing applications for availability and resiliency](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/testing)

<br><br>

### WATS-2 - エラーを処理するためのロジックをワークロードに組み込むことを検討します

**Category: Application Resilience**

**Impact: High**

**Recommendation/Guidance**

分散システムでは、アプリケーションがエラーから回復できることを確認することが重要です。アプリケーションをテストしてエラーや障害を防ぐことはできますが、さまざまな問題に備える必要があります。テストで常にすべてをキャッチできるとは限らないため、エラーを処理し、潜在的な障害を防ぐ方法を理解する必要があります。

基盤となるクラウドインフラストラクチャやサードパーティのランタイムの依存関係など、分散システム内の多くのものは、制御範囲外であり、テストする手段の範囲外です。いずれ何かが失敗することは確実なので、備えが必要です。

キーポイント:

- 再試行ロジックを実装して、一時的なアプリケーション障害と、内部または外部の依存関係を持つ一時的な障害を処理します。
- アプリケーションの再試行ロジックの問題や障害を明らかにする。
- 要求のタイムアウトを構成して、コンポーネント間の呼び出しを管理します。
- ロード バランサーと Traffic Manager の正常性プローブを構成してテストします。
- アプリケーション データ ストア間で読み取り操作と更新操作を分離します。

**Resources**

- [Error handling for resilient applications in Azure](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/app-design-error-handling)

<br><br>

### WATS-3 - ディザスタリカバリテストを定期的に実行します

**Category: Disaster Recovery**

**Impact: High**

**Recommendation/Guidance**

ディザスタリカバリは、壊滅的な損失の後にアプリケーションの機能を復元するプロセスです。
クラウド環境では、障害が発生することを前もって認識しています。目標は、障害を完全に防ごうとするのではなく、1 つのコンポーネントに障害が発生した場合の影響を最小限に抑えることです。テストは、これらの影響を最小限に抑える方法の 1 つです。アプリケーションのテストは可能な限り自動化する必要がありますが、失敗した場合に備える必要もあります。障害が発生した場合は、バックアップとリカバリの戦略が重要になります。

障害発生時の機能低下に対する許容度は、アプリケーションごとに異なるビジネス上の決定事項です。一部のアプリケーションが一時的に使用不可になったり、機能が制限されたり、処理が遅れたりして部分的に使用可能になることは許容できる場合があります。他のアプリケーションでは、機能の低下は許容されません。

キーポイント

- 主要な障害シナリオを使用して、ディザスタリカバリ計画を定期的に作成し、テストします。
- ほとんどのアプリケーションを機能制限して実行するためのディザスタリカバリ戦略を設計します。
- アプリケーションのビジネス要件と状況に合わせたバックアップ戦略を設計します。
- フェイルオーバーとフェイルバックのステップとプロセスを自動化します。
- フェールオーバーとフェールバックのアプローチを少なくとも 1 回は正常にテストして検証します。

**Resources**

- [Backup and disaster recovery for Azure applications](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/backup-and-recovery)

<br><br>

### WATS-4 - カオス エンジニアリングを使用して Azure アプリケーションをテストします

**Category: Application Resilience**

**Impact: Medium**

**Recommendation/Guidance**

理想的には、カオスの原則を継続的に適用する必要があります。ソフトウェアとハードウェアが稼働する環境は常に変化しているため、変化を監視することが重要です。コンポーネントに常にストレスや障害をかけることで、小さな問題が他の多くの要因によって悪化する前に、問題を早期に発見することができます。

カオスエンジニアリングの原則を適用するのは、次のような場合です。

- 新しいコードをデプロイします。
- 依存関係を追加します。
- 使用パターンの変化を観察します。
- 問題を軽減します。

**Resources**

- [Use chaos engineering to test Azure applications](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/chaos-engineering)

<br><br>

### WATS-5 - アプリケーションの障害回復性をテストします

**Category: Application Resilience**

**Impact: High**

**Guidance**

高可用性は、データベース アプリケーションに対して透過的に機能する SQL Database プラットフォームの基本的な部分です。ただし、計画的または計画外のイベント中に開始された自動フェールオーバー操作が、運用環境にデプロイする前にアプリケーションにどのような影響を与えるかをテストすることをお勧めします。フェールオーバーを手動でトリガーするには、特別な API を呼び出してデータベースまたはエラスティック プールを再起動します。

ゾーン冗長サーバーレスまたはプロビジョニングされた汎用データベースまたはエラスティック プールの場合、API 呼び出しにより、クライアント接続は、古いプライマリの可用性ゾーンとは異なる可用性ゾーン内の新しいプライマリにリダイレクトされます。そのため、フェイルオーバーが既存のデータベースセッションにどのような影響を与えるかをテストするだけでなく、ネットワークレイテンシの変化によってエンドツーエンドのパフォーマンスが変化するかどうかを検証することもできます。再起動操作は煩わしく、多数のフェールオーバー呼び出しがプラットフォームに負荷をかける可能性があるため、データベースまたはエラスティック プールごとに 15 分ごとに 1 つのフェールオーバー呼び出しのみが許可されます。

**Resources**

- [Test application fault resiliency](https://learn.microsoft.com/ja-jp/azure/azure-sql/database/high-availability-sla?view=azuresql&tabs=azure-powershell#testing-application-fault-resiliency)

<br><br>
