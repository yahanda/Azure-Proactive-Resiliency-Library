+++
title = "Storage Accounts (Blob/Azure Data Lake Storage Gen2)"
description = "Best practices and resiliency recommendations for Storage Account and associated resources."
date = "4/13/23"
author = "dost"
msAuthor = "dost"
draft = false
+++

The presented resiliency recommendations in this guidance include Storage Account and associated settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for Storage Account and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                |     Category      | Impact |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:--------------------:|
| [ST-1 - Ensure that storage accounts are zone or region redundant](#st-1---ensure-that-storage-accounts-are-zone-or-region-redundant)                                   |   Availability    |  High  | Verified |          Yes         |
| [ST-2 - Do not use classic storage accounts](#st-2---do-not-use-classic-storage-accounts)                                                                                                       |    Governance     |  High  | Verified |         Yes          |
| [ST-3 - Ensure performance tier is set as per workload](#st-3---ensure-performance-tier-is-set-as-per-workload)                                                                               | System Efficiency | Medium | Verified |          No          |
| [ST-5 - Enable soft delete for recovery of data](#st-5---enable-soft-delete-for-recovery-of-data)                                                                                             | Disaster Recovery | Medium | Verified |          No          |
| [ST-6 - Enable versioning for accidental modification and keep the number of versions below 1000](#st-6---enable-versioning-for-accidental-modification-and-keep-the-number-of-versions-below-1000) | Disaster Recovery | Low | Verified |          No          |
| [ST-7 - Consider enabling point-in-time restore for standard general purpose v2 accounts with flat namespace](#st-7---consider-enabling-point-in-time-restore-for-standard-general-purpose-v2-accounts-with-flat-namespace)                                                         | Disaster Recovery |  Low   | Verified |          No          |
| [ST-8 - Monitor all blob storage accounts](#st-8---monitor-all-blob-storage-accounts)                                                               |    Monitoring     |  Low   | Verified |          No          |
| [ST-9 - Consider upgrading legacy storage accounts to v2 storage accounts](#st-9---consider-upgrading-legacy-storage-accounts-to-v2-storage-accounts)                                                               | System Efficiency | Low | Verified |         Yes          |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ST-1 - ストレージ アカウントがゾーンまたはリージョン冗長であることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

冗長性により、ストレージ アカウントは、障害が発生した場合でも可用性と持続性の目標を確実に満たすことができます。シナリオに最適な冗長性オプションを決定するときは、コストの削減と可用性の向上のトレードオフを考慮してください。
ローカル冗長ストレージ (LRS) は、最も低コストの冗長性オプションであり、他のオプションと比較して持続性が最も低くなります。Microsoft では、ゾーン冗長ストレージ (ZRS)、geo 冗長ストレージ (GRS)、または geo ゾーン冗長ストレージ (GZRS) を使用して、可用性ゾーンまたはリージョンが使用できなくなった場合にストレージ アカウントを使用できるようにすることをお勧めします。


**Resources**

- [Azure Storage redundancy](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-redundancy)
- [Change the redundancy configuration for a storage account](https://learn.microsoft.com/ja-jp/azure/storage/common/redundancy-migration)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-1/st-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-2 - クラシック ストレージ アカウントを使用しないようにします

**Category: Governance**

**Impact: High**

**Guidance**

クラシック ストレージ アカウントは、2024 年 8 月 31 日に完全に廃止されます。クラシック ストレージ アカウントをお持ちの場合は、今すぐ移行の計画を開始してください。

**Resources**

- [Azure classic storage accounts retirement announcement](https://azure.microsoft.com/ja-jp/updates/classic-azure-storage-accounts-will-be-retired-on-31-august-2024/)
- [Migrate your classic storage accounts to Azure Resource Manager](https://learn.microsoft.com/ja-jp/azure/storage/common/classic-account-migration-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-2/st-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-3 - パフォーマンス レベルがワークロードごとに設定されていることを確認します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

ワークロードのシナリオに適したストレージ パフォーマンス レベルを使用することを検討してください。各ワークロード シナリオには適切なパフォーマンス レベルが必要であり、ストレージの使用量に基づいて適切なパフォーマンス レベルを選択することが重要です。

**Resources**

- [Types of storage accounts](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-account-overview#types-of-storage-accounts)
- [Scalability and performance targets for standard storage accounts](https://learn.microsoft.com/ja-jp/azure/storage/common/scalability-targets-standard-account)
- [Performance and scalability checklist for Blob storage](https://learn.microsoft.com/ja-jp/azure/storage/blobs/storage-performance-checklist)
- [Scalability and performance targets for Blob storage](https://learn.microsoft.com/ja-jp/azure/storage/blobs/scalability-targets)
- [Premium block blob storage accounts](https://learn.microsoft.com/ja-jp/azure/storage/blobs/storage-blob-block-blob-premium)
- [Scalability targets for premium block blob storage accounts](https://learn.microsoft.com/ja-jp/azure/storage/blobs/scalability-targets-premium-block-blobs)
- [Scalability and performance targets for premium page blob storage accounts](https://learn.microsoft.com/ja-jp/azure/storage/blobs/scalability-targets-premium-page-blobs)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-3/st-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-5 - データの回復のための論理的な削除を有効にします

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

論理的な削除オプションを使用すると、誤って削除された場合にデータを回復できます。さらに、ロックはストレージアカウントを誤って削除するのを防ぎます。

**Resources**

- [Soft delete detail docs](https://learn.microsoft.com//azure/storage/blobs/soft-delete-blob-enable?tabs=azure-portal )

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-5/st-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-6 - 偶発的な変更に対してバージョン管理を有効にし、バージョンの数を 1000 未満に保ちます

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

偶発的な変更や削除からデータを回復するために、バージョン管理を有効にすることを検討してください。
BLOB あたりのバージョン数が多いと、BLOB の一覧表示操作の待機時間が長くなる可能性があります。Microsoft では、BLOB あたり 1,000 未満のバージョンを維持することをお勧めします。ライフサイクル管理を使用して、古いバージョンを自動的に削除できます。

**Resources**

- [Blob versioning](https://learn.microsoft.com/ja-jp/azure/storage/blobs/versioning-overview )

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-6/st-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-7 - フラットな名前空間を持つ標準の汎用 v2 アカウントに対してポイントインタイム リストアを有効にすることを検討してください

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

フラットな名前空間を持つ標準の汎用 v2 アカウントに対してポイントインタイム リストアを有効にすることを検討してください。ポイントインタイム リストアは、ブロック BLOB データを以前の状態に復元できるようにすることで、偶発的な削除や破損から保護します。

**Resources**

- [Point-in-time restore for block blobs](https://learn.microsoft.com/ja-jp/azure/storage/blobs/point-in-time-restore-overview)
- [Perform a point-in-time restore on block blob data](https://learn.microsoft.com/ja-jp/azure/storage/blobs/point-in-time-restore-manage?tabs=portal)

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-7/st-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-8 - すべての BLOB ストレージ アカウントを監視します

**Category: Monitoring**

**Impact: Low**

**Guidance**

Azure リソースに依存する重要なアプリケーションやビジネス プロセスがある場合は、システムを監視してアラートを取得する必要があります。
リソース ログは、診断設定を作成し、ログを 1 つ以上の場所にルーティングするまで収集および保存されません。診断設定を作成するときは、収集するログのカテゴリを指定します。

**Resources**

- [Monitor Azure Blob Storage](https://learn.microsoft.com/ja-jp/azure/storage/blobs/monitor-blob-storage)
- [Best practices for monitoring Azure Blob Storage](https://learn.microsoft.com/ja-jp/azure/storage/blobs/blob-storage-monitoring-scenarios)

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-8/st-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-9 - レガシ ストレージ アカウントを v2 ストレージ アカウントにアップグレードすることを検討します

**Category: System Efficiency**

**Impact: Low**

**Guidance**

汎用 v2 アカウントは、最新の機能またはギガバイトあたりの最低価格を備えたほとんどのストレージ シナリオに推奨されます。レガシ アカウントの種類 (Standard 汎用 v1 と Blob Storage) は Microsoft では推奨されていませんが、特定のシナリオで使用される場合があります。
ドキュメントに記載されているシナリオ (クラシック互換性、トランザクション集中型など) を考慮し、該当する場合はレガシ ストレージ アカウントを v2 ストレージ アカウントにアップグレードしてください。

汎用 v1 または Blob ストレージ アカウントから汎用 v2 ストレージ アカウントへのアップグレードは簡単です。汎用 v2 ストレージ アカウントへのアップグレードに関連するダウンタイムやデータ損失のリスクはありません。汎用 v1 または Blob Storage アカウントから汎用 v2 へのアップグレードは永続的であり、元に戻すことはできません。

**Resources**

- [Legacy storage account types](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-account-overview#legacy-storage-account-types)
- [Upgrade to a general-purpose v2 storage account](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-account-upgrade)

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-9/st-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
