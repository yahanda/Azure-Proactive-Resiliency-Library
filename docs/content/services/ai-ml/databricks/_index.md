+++
title = "Azure Databricks"
description = "Best practices and resiliency recommendations for Azure Databricks and associated resources."
date = "10/09/23"
author = "oZakari"
msAuthor = "ztrocinski"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure Databricks and dependent resources and settings.

## Summary of Recommendation
{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                                |        Category        | Impact |  State   | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------:|:------:|:--------:|:-------------------:|
| [DBW-1 - Databricks runtime version is not latest and/or is not LTS version](#dbw-1---databricks-runtime-version-is-not-latest-or-is-not-lts-version)                                                                         |       Governance       | Medium | Verified |         No          |
| [DBW-2 - Use Databricks Pools](#dbw-2---use-databricks-pools)                                                                                                                                                                 |   System Efficiency    |  High  | Verified |         No          |
| [DBW-3 - Use SSD backed VMs for Worker VM Type and Driver type](#dbw-3---use-ssd-backed-vms-for-worker-vm-type-and-driver-type)                                                                                               |   System Efficiency    | Medium | Verified |         No          |
| [DBW-4 - Enable autoscaling for batch workloads](#dbw-4---enable-autoscaling-for-batch-workloads)                                                                                                                             |   System Efficiency    |  High  | Verified |         No          |
| [DBW-5 - Enable autoscaling for SQL warehouse](#dbw-5---enable-autoscaling-for-sql-warehouse)                                                                                                                                 |   System Efficiency    |  High  | Verified |         No          |
| [DBW-6 - Use Delta Live Tables enhanced autoscaling](#dbw-6---use-delta-live-tables-enhanced-autoscaling)                                                                                                                     |   System Efficiency    | Medium | Verified |         No          |
| [DBW-7 - Automatic Job Termination is enabled, ensure there are no user-defined local processes](#dbw-7---automatic-job-termination-is-enabled-ensure-there-are-no-user-defined-local-processes)                              |      Availability      | Medium | Verified |         No          |
| [DBW-8 - Enable Logging-Cluster log delivery](#dbw-8---enable-logging-cluster-log-delivery)                                                                                                                                   |       Monitoring       | Medium | Verified |         No          |
| [DBW-9 - Use Delta Lake for higher reliability](#dbw-9---use-delta-lake-for-higher-reliability)                                                                                                                               |      Availability      |  High  | Verified |         No          |
| [DBW-10 - Use Photon Acceleration](#dbw-10---use-photon-acceleration)                                                                                                                                                         |      Availability      |  Low   | Verified |         No          |
| [DBW-11 - Automatically rescue invalid or nonconforming data with Databricks Auto Loader or Delta Live Tables](#dbw-11---automatically-rescue-invalid-or-nonconforming-data-with-databricks-auto-loader-or-delta-live-tables) | Application Resilience |  Low   | Verified |         No          |
| [DBW-12 - Configure jobs for automatic retries and termination](#dbw-12---configure-jobs-for-automatic-retries-and-termination)                                                                                               |      Availability      |  High  | Verified |         No          |
| [DBW-13 - Use a scalable and production-grade model serving infrastructure](#dbw-13---use-a-scalable-and-production-grade-model-serving-infrastructure)                                                                       |   System Efficiency    |  High  | Verified |         No          |
| [DBW-14 - Use a layered storage architecture](#dbw-14---use-a-layered-storage-architecture)                                                                                                                                   | Application Resilience | Medium | Verified |         No          |
| [DBW-15 - Improve data integrity by reducing data redundancy](#dbw-15---improve-data-integrity-by-reducing-data-redundancy)                                                                                                   | Application Resilience |  Low   | Verified |         No          |
| [DBW-16 - Actively manage schemas](#dbw-16---actively-manage-schemas)                                                                                                                                                         |       Governance       | Medium | Verified |         No          |
| [DBW-17 - Use constraints and data expectations](#dbw-17---use-constraints-and-data-expectations)                                                                                                                             | Application Resilience |  Low   | Verified |         No          |
| [DBW-18 - Create regular backups](#dbw-18---create-regular-backups)                                                                                                                                                           |   Disaster Recovery    |  Low   | Verified |         No          |
| [DBW-19 - Recover from Structured Streaming query failures](#dbw-19---recover-from-structured-streaming-query-failures)                                                                                                       |      Availability      |  High  | Verified |         No          |
| [DBW-20 - Recover ETL jobs based on Delta time travel](#dbw-20---recover-etl-jobs-based-on-delta-time-travel)                                                                                                                 |   Disaster Recovery    | Medium | Verified |         No          |
| [DBW-21 - Use Databricks Workflows and built-in recovery](#dbw-21---use-databricks-workflows-and-built-in-recovery)                                                                                                           |   Disaster Recovery    |  Low   | Verified |         No          |
| [DBW-22 - Configure a disaster recovery pattern](#dbw-22---configure-a-disaster-recovery-pattern)                                                                                                                             |   Disaster Recovery    |  High  | Preview  |         No          |
| [DBW-23 - Automate deployments and workloads](#dbw-23---automate-deployments-and-workloads)                                                                                                                                   |       Automation       |  High  | Preview  |         No          |
| [DBW-24 - Set up monitoring, alerting, and logging](#dbw-24---set-up-monitoring-alerting-and-logging)                                                                                                                         |       Monitoring       |  High  | Preview  |         No          |
| [DBW-25 - Deploy workspaces in separate Subscriptions](#dbw-25---deploy-workspaces-in-separate-subscriptions)                                                                                                                 |   System Efficiency    |  High  | Preview  |         No          |
| [DBW-26 - Isolate each workspace in its own Vnet](#dbw-26---isolate-each-workspace-in-its-own-vnet)                                                                                                                           |   System Efficiency    |  High  | Preview  |         No          |
| [DBW-27 - Do not Store any Production Data in Default DBFS Folders](#dbw-27---do-not-store-any-production-data-in-default-dbfs-folders)                                                                                       |      Availability      |  High  | Preview  |         No          |
| [DBW-28 - Do not use Azure Sport VMs for critical Production workloads](#dbw-28---do-not-use-azure-sport-vms-for-critical-production-workloads)                                                                               |      Availability      |  High  | Preview  |         No          |
| [DBW-29 - Migrate Legacy Workspaces](#dbw-29---migrate-legacy-workspaces)                                                                               |      Availability      |  High  | Preview  |         No          |
| [DBW-30 - Define alternate VM SKUs](#dbw-30---define-alternate-vm-skus)                                                                               |      System Efficiency      |  Medium  | Preview  |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### DBW-1 - Databricks ランタイムのバージョンが最新ではないか、LTS バージョンではありません

**Category: Governance**

**Impact: Medium**

**Guidance**

12.2 LTS 以降を使用してください。Databricks では、次の順序でワークロードを移行することをお勧めします。:

- ワークロードが現在 Databricks Runtime 11.3 LTS 以降で実行されている場合は、この記事の後半で説明するように、最新バージョンの Databricks Runtime 12.x に直接移行できます。
- ワークロードが現在 Databricks Runtime 11.3 LTS 以下で実行されている場合は、次の操作を行います。
  - 最初に Databricks Runtime 11.3 LTS に移行します。Databricks Runtime 11.x 移行ガイドを参照してください。
  - この記事のガイダンスに従って、Databricks Runtime 11.3 LTS から最新バージョンの Databricks Runtime 12.x に移行します。

**Resources**

- [Databricks runtime support lifecycles](https://learn.microsoft.com/ja-jp/azure/databricks/release-notes/runtime/databricks-runtime-ver)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-1/dbw-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-2 - Databricks プールを使用します

**Category: System Efficiency**

**Impact: High**

**Guidance**

Databricks プールはサービスの標準機能であり、VM をオンデマンドでスピンアップするのではなく、事前にプロビジョニングすることで、クラスターの開始時またはスケーリング時の "プロビジョニング" エラーのリスクを大幅に軽減できます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-2/dbw-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-3 - Worker VM Type と Driver type に SSD ベースの VM を使用します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

Premium 対応の仮想マシンで標準ハード ディスクを使用していることが確認されたため、Standard HDD ディスクを Standard SSD または Premium ディスクにアップグレードすることを検討することをお勧めします。すべてのオペレーティング システム ディスクとデータ ディスクに Premium Storage を使用する単一インスタンス仮想マシンについては、マイクロソフトは、仮想マシンの接続性が 99.9% 以上であることを保証します。アップグレードを決定する際には、これらの要素を考慮してください。1 つ目は、アップグレードには VM の再起動が必要であり、このプロセスが完了するまでに 3 分から 5 分かかることです。2 つ目は、一覧にある VM がミッション クリティカルな運用 VM である場合は、Premium ディスクのコストに対して可用性の向上を評価することです。

- Premium SSD ディスクは、I/O 集中型のアプリケーションや運用ワークロードに対して、高パフォーマンスで待機時間の短いディスク サポートを提供します。
- Standard SSD ディスクは、低い IOPS レベルで一貫したパフォーマンスを必要とするワークロード向けに最適化された、コスト効率の高いストレージ オプションです。
- Standard HDD ディスクは、開発/テストのシナリオと重要度の低いワークロードに最小のコストで使用します。

Standard SSD は、一部の運用ワークロードでも使用できます。

**Resources**

- [Azure managed disk types](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-types#premium-ssd)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-3/dbw-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-4 - バッチ ワークロードの自動スケーリングを有効にします

**Category: System Efficiency**

**Impact: High**

**Guidance**

自動スケーリングを使用すると、ワークロードに基づいてクラスターのサイズを自動的に変更できます。自動スケーリングは、コストとパフォーマンスの両方の観点から、多くのユース ケースとシナリオにメリットをもたらします。このドキュメントでは、自動スケーリングを使用するかどうか、および最大のメリットを得る方法を決定するための考慮事項について説明します。

ストリーミング ワークロードの場合、Databricks では、自動スケーリングで Delta Live Tables を使用することをお勧めします。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices#enable-autoscaling-for-batch-workloadss)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-4/dbw-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-5 - SQL warehouse の自動スケーリングを有効にします

**Category: System Efficiency**

**Impact: High**

**Guidance**

SQL ウェアハウスのスケーリング パラメーターは、ウェアハウスに送信されたクエリが分散されるクラスターの最小数と最大数を設定します。デフォルトは、最小で 1 つ、最大で 1 つのクラスターです。

特定のウェアハウスでより多くの同時ユーザーを処理するには、クラスター数を増やします。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices#enable-autoscaling-for-sql-warehouse)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-5/dbw-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-6 - Delta Live Tables の拡張自動スケーリングを使用します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

Databricks の拡張自動スケーリングでは、パイプラインのデータ処理待機時間への影響を最小限に抑えながら、ワークロード ボリュームに基づいてクラスター リソースを自動的に割り当てることで、クラスターの使用率を最適化します。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)
- [Databricks enhanced autoscaling](https://learn.microsoft.com/ja-jp/azure/databricks/delta-live-tables/settings#use-autoscaling-to-increase-efficiency-and-reduce-resource-usage)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-6/dbw-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-7 - ジョブの自動終了が有効になっており、ユーザー定義のローカルプロセスがないことを確認します

**Category: Availability**

**Impact: Medium**

**Guidance**

クラスタ リソースを節約するために、クラスタを終了できます。終了したクラスターの構成は、後で再利用 (ジョブの場合は自動開始) できるように保存されます。クラスタを手動で終了することも、指定した非アクティブな期間が経過すると自動的に終了するようにクラスタを設定することもできます。終了したクラスタの数が 150 を超えると、最も古いクラスタが削除されます。
また、クラスタの自動終了を設定することもできます。クラスターの作成時に、クラスターを終了するまでの非アクティブ期間を分単位で指定できます。
ただし、自動終了機能では、ユーザー定義のローカル プロセスではなく、Spark ジョブのみが監視されます。そのため、すべての Spark ジョブが完了すると、ローカル プロセスが実行されている場合でも、クラスターが終了する可能性があります。

**Resources**

- [Best practices for reliability?](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-7/dbw-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-8 - Logging-Cluster ログ配信を有効にします

**Category: Monitoring**

**Impact: Medium**

**Guidance**

クラスターを作成するときに、Spark ドライバー ノード、ワーカー ノード、およびイベントのログを配信する場所を指定できます。ログは 5 分ごとに配信され、選択した宛先に 1 時間ごとにアーカイブされます。クラスターが終了すると、Azure Databricks は、クラスターが終了するまでに生成されたすべてのログを配信することを保証します。

ログの宛先は、クラスター ID によって異なります。指定した宛先が dbfs:/cluster-log-delivery の場合、0630-191345-leap375 のクラスター ログは dbfs:/cluster-log-delivery/0630-191345-leap375 に配信されます。

**Resources**

- [Create a cluster](https://learn.microsoft.com/ja-jp/azure/databricks/clusters/configure#cluster-log-delivery)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-8/dbw-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-9 - Delta Lake を使用して信頼性を高めます

**Category: Availability**

**Impact: High**

**Guidance**

Delta Lake は、データ レイクに信頼性をもたらすオープン ソースのストレージ形式です。Delta Lake は、ACID トランザクション、スキーマの適用、スケーラブルなメタデータ処理を提供し、ストリーミングとバッチのデータ処理を統合します。Delta Lake は、既存のデータ レイク上で実行され、Apache Spark API と完全に互換性があります。Databricks 上の Delta Lake では、ワークロード パターンに基づいて Delta Lake を構成できます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-9/dbw-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-10 - Photon アクセラレーションを使用します

**Category: Availability**

**Impact: Low**

**Guidance**

Apache Spark は、Databricks レイクハウスのコンピューティング エンジンとして、回復力のある分散データ処理に基づいています。内部 Spark タスクが期待どおりの結果を返さない場合、Apache Spark は不足しているタスクを自動的に再スケジュールし、ジョブ全体の実行を続行します。これは、短いネットワークの問題やスポット VM の失効など、コード外のエラーに役立ちます。SQL API と Spark DataFrame API の両方を使用すると、エンジンにこの回復性が組み込まれます。

Databricks レイクハウスでは、完全に C++ で記述されたネイティブ ベクトル化エンジンである Photon は、Apache Spark API と互換性のあるハイ パフォーマンス コンピューティングです。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices#use-apache-spark-or-photon-for-distributed-compute)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-10/dbw-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-11 - Databricks Auto Loader または Delta Live Tables を使用して、無効なデータや不適合なデータを自動的に復旧します

**Category: Application Resilience**

**Impact: Low**

**Guidance**

無効なデータや不適合なデータは、確立されたデータ形式に依存するワークロードのクラッシュにつながる可能性があります。プロセス全体のエンドツーエンドの回復性を高めるには、インジェスト時に無効なデータや不適合なデータを除外することをお勧めします。レスキューされたデータをサポートすることで、取り込みやETL中にデータを失ったり見逃したりすることがなくなります。復旧されたデータ列には、指定されたスキーマに存在しないか、型の不一致があったか、レコードまたはファイルの列の大文字と小文字がスキーマの大文字と小文字が一致しなかったために、解析されなかったデータが含まれます。

- Databricks 自動ローダー: 自動ローダーは、ファイルのインジェストをストリーミングするための理想的なツールです。JSON と CSV のレスキューされたデータをサポートします。
- Delta Live Tables: 回復性のためのワークフローを構築するための別のオプションは、品質制約のある Delta Live Tables を使用することです。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-11/dbw-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-12 - 自動再試行と自動終了のジョブを構成します

**Category: Availability**

**Impact: High**

**Guidance**

バッチ推論とストリーミング推論の場合は、Databricks ジョブと MLflow を使用してモデルを Apache Spark UDF としてデプロイし、ジョブのスケジュール設定、再試行、自動スケーリングなどを活用します。
モデル提供は、スケーラブルで実稼働グレードのモデルリアルタイムサービスインフラストラクチャを提供します。MLflow を使用して機械学習モデルを処理し、REST API エンドポイントとして公開します。この機能ではサーバーレス コンピューティングが使用されるため、エンドポイントと関連するコンピューティング リソースは Databricks クラウド アカウントで管理および実行されます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-12/dbw-12.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-13 - スケーラブルで実稼働グレードのモデル提供用インフラストラクチャを使用します

**Category: System Efficiency**

**Impact: High**

**Guidance**

バッチ推論とストリーミング推論の場合は、Databricks ジョブと MLflow を使用してモデルを Apache Spark UDF としてデプロイし、ジョブのスケジュール設定、再試行、自動スケーリングなどを活用します。
モデル提供は、スケーラブルで実稼働グレードのモデルリアルタイムサービスインフラストラクチャを提供します。MLflow を使用して機械学習モデルを処理し、REST API エンドポイントとして公開します。この機能ではサーバーレス コンピューティングが使用されるため、エンドポイントと関連するコンピューティング リソースは Databricks クラウド アカウントで管理および実行されます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-13/dbw-13.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-14 - 階層型ストレージアーキテクチャを使用します

**Category: Application Resilience**

**Impact: Medium**

**Guidance**

階層化されたアーキテクチャを作成し、データが階層を通過するにつれてデータ品質が向上するようにすることで、データをキュレーションします。一般的な階層化のアプローチは次のとおりです。

未加工レイヤー (ブロンズ): ソース データはレイクハウスの最初のレイヤーに取り込まれ、そこに保持する必要があります。すべての下流データが未加工レイヤーから作成されたら、必要に応じて、このレイヤーから後続のレイヤーを再構築できます。

キュレーションされたレイヤー (シルバー): 2 番目のレイヤーの目的は、クレンジング、絞り込み、フィルター処理、および集計されたデータを保持することです。このレイヤーの目標は、すべての役割と機能にわたる分析とレポートのための健全で信頼性の高い基盤を提供することです。

最終層 (ゴールド): 3 番目の層は、ビジネスまたはプロジェクトのニーズに基づいて作成されます。他の部署やプロジェクトに対してデータ製品として異なるビューを提供し、セキュリティ ニーズに関するデータ (匿名化されたデータなど) を準備したり、パフォーマンスを最適化したり (事前集計されたビューなど) したりします。このレイヤーのデータ製品は、ビジネスにとっての事実と見なされます。

最後のレイヤーには、高品質のデータのみを含める必要があり、ビジネスの観点から完全に信頼できるものでなければなりません。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-14/dbw-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-15 - データの冗長性を削減することでデータの整合性を向上させます

**Category: Application Resilience**

**Impact: Low**

**Guidance**

データをコピーまたは複製すると、データの冗長性が生じ、整合性が失われ、データ系列が失われ、多くの場合、それぞれのアクセス許可が異なります。これにより、レイクハウス内のデータの品質が低下します。データの一時的または使い捨て用のデータのコピーは、それ自体では有害ではなく、俊敏性、実験、イノベーションを促進するために必要になる場合があります。ただし、これらのコピーが運用可能になり、ビジネス上の意思決定に定期的に使用されるようになると、データのサイロ化が進みます。これらのデータサイロが同期しなくなると、データの整合性と品質に重大な悪影響を及ぼし、「どのデータセットがマスターなのか」や「データセットは最新か」などの疑問が生じます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-15/dbw-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-16 - スキーマをアクティブに管理します

**Category: Governance**

**Impact: Medium**

**Guidance**

制御されていないスキーマ変更は、無効なデータや、これらのデータ・セットを使用するジョブの失敗につながる可能性があります。Databricks には、スキーマを検証して適用するためのいくつかの方法があります。

- Delta Lake は、スキーマのバリエーションを自動的に処理して、取り込み中に不適切なレコードが挿入されるのを防ぐことで、スキーマの検証とスキーマの適用をサポートします。
- 自動ローダーは、データの処理中に新しい列の追加を検出します。既定では、新しい列を追加すると、ストリームは UnknownFieldException で停止します。自動ローダーでは、スキーマの進化のためにいくつかのモードがサポートされています。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-16/dbw-16.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-17 - 制約とデータの期待値を使用します

**Category: Application Resilience**

**Impact: Low**

**Guidance**

Delta テーブルでは、テーブルに追加されるデータの品質と整合性が自動的に検証されるようにする標準の SQL 制約管理句がサポートされています。制約に違反すると、Delta Lake は InvariantViolationException エラーをスローして、新しいデータを追加できないことを通知します。「Azure Databricks の制約」を参照してください。

この処理をさらに改善するために、Delta Live Tables では、期待値: 期待値は、データ セットの内容に対するデータ品質の制約を定義します。期待値は、説明、不変条件、およびレコードが不変条件に失敗した場合に実行するアクションで構成されます。クエリへの期待は、Python デコレーターまたは SQL 制約句を使用します。「Delta Live Tables を使用したデータ品質の管理」を参照してください。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices#use-constraints-and-data-expectations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-17/dbw-17.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-18 - 定期的なバックアップを作成します

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

障害から回復するには、定期的なバックアップを利用できる必要があります。Databricks Labs プロジェクトの移行を使用すると、ワークスペース管理者は、ワークスペースのほとんどの資産をエクスポートしてバックアップを作成できます (このツールはバックグラウンドで Databricks CLI/API を使用します)。「Databricks 移行ツール」を参照してください。バックアップは、ワークスペースの復元、または移行の場合の新しいワークスペースへのインポートに使用できます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices#create-regular-backups)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-18/dbw-18.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-19 - 構造化ストリーミングのクエリ エラーから復旧します

**Category: Availability**

**Impact: High**

**Guidance**

構造化ストリーミングは、ストリーミング クエリのフォールト トレランスとデータの一貫性を提供します。Azure Databricks ワークフローを使用すると、障害発生時に自動的に再起動するように構造化ストリーミング クエリを簡単に構成できます。再開されたクエリは、失敗したクエリが中断したところから続行されます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices#recover-from-structured-streaming-query-failures)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-19/dbw-19.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-20 - Delta のタイム トラベルに基づいて ETL ジョブを復旧します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

徹底的なテストを行ったとしても、本番環境のジョブは失敗したり、予期しないデータや無効なデータが生成されたりする可能性があります。追加のジョブにより問題の原因を理解し、この問題を最初に引き起こしたパイプラインを修正すれば、これを解決できる場合があります。ただし、多くの場合、この作業は単純ではなく、それぞれのジョブをロールバックする必要が生じます。Delta Time Travel を使用すると、ユーザーは変更を古いバージョンまたはタイムスタンプに簡単にロールバックし、パイプラインを修復し、修正されたパイプラインを再起動できます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices#recover-etl-jobs-based-on-delta-time-travel)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-20/dbw-20.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-21 - Databricks Workflows と組み込みの復旧機能を使用します

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

Databricks ワークフローは復旧用に構築されています。マルチタスク ジョブのタスク (および、すべての依存タスク) が失敗した場合、Azure Databricks Workflows では実行のマトリックス ビューが提供され、失敗の原因となった問題を調べることができます。「ジョブの実行の表示」を参照してください。ネットワークが短い問題であった場合でも、データの実際の問題であった場合でも、Azure Databricks Workflows で修正して修復実行を開始できます。失敗したタスクと依存するタスクのみを実行し、以前の実行から成功した結果を保持するため、時間とコストを節約できます。

**Resources**

- [Best practices for reliability](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/reliability/best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-21/dbw-21.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-22 - ディザスター リカバリー パターンを構成します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

明確なディザスター リカバリー パターンは、Azure Databricks のようなクラウドネイティブなデータ分析プラットフォームにとって重要です。一部の企業では、ハリケーンや地震などの地域的な災害やその他の発生源によって引き起こされるかどうかにかかわらず、地域のサービス全体のクラウドサービスプロバイダーが停止するというまれなケースでも、データチームがDatabricksプラットフォームを使用できることが重要です。

**Resources**

- [Azure Databricks Best Practices](https://github.com/Azure/AzureDatabricksBestPractices/tree/master)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-22/dbw-22.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-23 - デプロイとワークロードを自動化します

**Category: Automation**

**Impact: High**

**Guidance**

Databricks Terraform プロバイダーは、柔軟で強力なツールを使用して、Azure Databricks ワークスペースと関連するクラウド インフラストラクチャを管理します。Databricks Terraform プロバイダーの目標は、すべての Azure Databricks REST API をサポートし、データ プラットフォームのデプロイと管理の最も複雑な側面の自動化をサポートすることです。Databricks Terraform プロバイダーは、クラスターとジョブを確実にデプロイして管理し、Azure Databricks ワークスペースをプロビジョニングし、データ アクセスを構成するための推奨ツールです。

**Resources**

- [Best practices for operational excellence](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/operational-excellence/best-practices#2-automate-deployments-and-workloads)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-23/dbw-23.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-24 - 監視、アラート、ログ記録を設定します

**Category: Monitoring**

**Impact: High**

**Guidance**

Databricks Terraform プロバイダーは、柔軟で強力なツールを使用して、Azure Databricks ワークスペースと関連するクラウド インフラストラクチャを管理します。Databricks Terraform プロバイダーの目標は、すべての Azure Databricks REST API をサポートし、データ プラットフォームのデプロイと管理の最も複雑な側面の自動化をサポートすることです。Databricks Terraform プロバイダーは、クラスターとジョブを確実にデプロイして管理し、Azure Databricks ワークスペースをプロビジョニングし、データ アクセスを構成するための推奨ツールです。

**Resources**

- [Best practices for operational excellence](https://learn.microsoft.com/ja-jp/azure/databricks/lakehouse-architecture/operational-excellence/best-practices#system-monitoring)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-24/dbw-24.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-25 - ワークスペースを個別のサブスクリプションにデプロイします

**Category: System Efficiency**

**Impact: High**

**Guidance**

Customers commonly partition workspaces based on teams or departments and arrive at that division naturally. But it is also important to partition keeping Azure Subscription and ADB Workspace limits in mind.

**Resources**

- [Azure Databricks Best Practices](https://github.com/Azure/AzureDatabricksBestPractices/blob/master/toc.md#deploy-workspaces-in-multiple-subscriptions-to-honor-azure-capacity-limits)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-25/dbw-25.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-26 - 各ワークスペースを独自の VNet に分離します

**Category: System Efficiency**

**Impact: High**

**Guidance**

関連付けられているサブネット ペアを他のワークスペースから分離することで、VNet に複数のワークスペースをデプロイできますが、どの VNet にも 1 つのワークスペースのみをデプロイすることをお勧めします。これを行うことは、ADBのワークスペースレベルの分離モデルと完全に一致しています。ほとんどの場合、組織では、vnet 内のプライベート アドレス空間がすべてのリソースで共有されるため、DNS などの共通のネットワーク リソースを共有できるように、複数のワークスペースを同じ VNet に配置することを検討します。ハブ アンド スポーク モデルに従い、Vnet ピアリングを使用してワークスペース VNet のプライベート IP 空間を拡張することで、ワークスペースを分離しながら同じことを簡単に実現できます。

**Resources**

- [Azure Databricks Best Practices](https://github.com/Azure/AzureDatabricksBestPractices/blob/master/toc.md#consider-isolating-each-workspace-in-its-own-vnet)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-26/dbw-26.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-27 - 既定の DBFS フォルダーに運用データを格納しないでください

**Category: Availability**

**Impact: High**

**Guidance**

この推奨事項は、セキュリティとデータの可用性に関する懸念によって推進されています。すべてのワークスペースには、主にライブラリや、Init スクリプトなどの他のシステムレベルの構成アーティファクトを格納するように設計された既定の DBFS が付属しています。次の理由により、運用データを格納しないでください。

- 既定の DBFS のライフサイクルは、ワークスペースに関連付けられています。ワークスペースを削除すると、既定の DBFS も削除され、その内容が完全に削除されます。
- この既定のフォルダーとその内容へのアクセスを制限することはできません。

**Resources**

- [Azure Databricks Best Practices](https://github.com/Azure/AzureDatabricksBestPractices/blob/master/toc.md#do-not-store-any-production-data-in-default-dbfs-foldersr)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-27/dbw-27.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-28 - 重要な運用ワークロードに Azure Sport VM を使用しないでください

**Category: Availability**

**Impact: High**

**Guidance**

Azure スポット VM は、高可用性と信頼性を必要とする重要な運用ワークロードには推奨されません。Azure スポット VM は、フォールト トレラントで中断を許容できるワークロード向けに設計されています。使用可能な容量は、サイズ、リージョン、時刻などによって異なります。Azure Spot Virtual Machines をデプロイする場合、使用可能な容量がある場合、Azure によって VM が割り当てられますが、これらの VM には SLA がありません。Azure Spot Virtual Machine では、高可用性は保証されません。Azure で容量の回復が必要になった時点で、Azure インフラストラクチャは 30 秒前に通知して Azure Spot Virtual Machines を削除します。

**Resources**

- [Use Azure Spot Virtual Machines](https://learn.microsoft.com/ja-jp/azure/virtual-machines/spot-vms)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-28/dbw-28.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### DBW-29 - Migrate Legacy Workspaces

**Category: Availability**

**Impact: High**

**Guidance**

Azure Databricks initially launched with shared control plane, where some regions shared control plane resources with another region. This shared control plane model then evolved to dedicated in-region control planes (e.g. North Europe, Central US, East US) to ensure a regional outage does not impact customer workspaces in other regions.

Regions that now have their dedicated control plane have workspaces running in two configurations:

- Legacy Workspaces - these are workspaces created before the dedicated control plane was available.
- Workspaces - these are workspaces created after the dedicated control plane was available.

The path for migrating legacy workspaces to use the in-region control plane is to **redeploy**.

Review the list of network addresses used in each region in the Microsoft documentation and determine which regions are sharing a control plane. For example, we can look up Canada East in the table and see that the address for its SCC relay is "tunnel.canadacentral.azuredatabricks.net". Since the relay address is in Canada Central, we know that "Canada East" is using the control plane in another region.

Some regions list two different addresses in the Azure Databricks Control plane networking table. For example, North Europe lists both "tunnel.westeurope.azuredatabricks.net" and "tunnel.northeuropec2.azuredatabricks.net" for the SCC relay address. This is because North Europe once shared the West Europe control plane, but it now has its own independent control plane. There are still some old, legacy workspaces in North Europe tied to the old control plane, but all workspaces created since the switch-over will be using the new control plane.

Once a new Azure Databricks workspace is created, it should be configured to match the original legacy workspace.  Databricks, Inc.
recommends that customers use the Databricks Terraform Exporter for both the initial copy and for maintaining the workspace. However, this exporter is still in the experimental phase. For customers that do not trust experimental projects or for customers that do not want to use Terraform, they can use the "Migrate" tool that Databricks, Inc. maintains with GitHub. This is a collection of scripts that will export all of the objects (notebooks, cluster definitions, metadata, *etc.*) from one workspace and then import them to another workspace.  Customers can use the "Migrate" tool to initially populate the new
workspace and then use their CI/CD deployment process to keep the workspace in sync.

Pro Tip: If you need to determine where the control plane is located for a particular Databricks workspace, you can use the "nslookup" console command on Windows or Linux with the workspace address.  The result will tell you where the control plane is located.

**Resources**

- [Azure Databricks regions - IP addresses and domains](https://learn.microsoft.com/azure/databricks/resources/supported-regions#--ip-addresses-and-domains)
- [Migrate - maintained by Databricks Inc.](https://github.com/databrickslabs/migrate)
- [Databricks Terraform Exporter - maintained by Databricks Inc. (Experimental)](https://registry.terraform.io/providers/databricks/databricks/latest/docs/guides/experimental-exporter)

<br><br>

### DBW-30 - Define alternate VM SKUs

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

Azure Databricks availability planning should include plans for swapping VM SKUs based on capacity constraints.

Azure Databricks creates its VMs as regional VMs and depends on Azure to choose the best availability zone for the VM.  In the past, there have been rare instances where compute can not be allocated due to zonal or regional VM constraints.  Thus, resulting in a "CLOUD PROVIDER" error.

In these situations, customers have two options:

- Use Databricks Pools.  To manage costs, customers should be careful when selecting the size of their pools. They will have to pay for the Azure VMs even when they are idle in the pool.  Databricks pool can contain only one SKU of VMs; you cannot mix multiple SKUs in the same pool. To reduce the number of pools that customers need to manage, they should settle on a few SKUs that will service their jobs instead of using a different VM
SKU for each job.
- Plan for alternative SKUs in their preferred region(s).

**Resources**

- [Compute configuration best practices](https://learn.microsoft.com/azure/databricks/compute/cluster-config-best-practices)
- [GPU-enabled compute](https://learn.microsoft.com/azure/databricks/compute/gpu)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/dbw-30/dbw-30.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
