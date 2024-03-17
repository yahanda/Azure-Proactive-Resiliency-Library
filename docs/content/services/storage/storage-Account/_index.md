+++
title = "Storage Account"
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
| [ST-1 - Ensure that Storage Account configuration is at least Zone redundant](#st-1---ensure-that-storage-account-configuration-is-at-least-zone-redundant)                                   |   Availability    |  High  | Preview |          Yes         |
| [ST-2 - Do not use classic storage account](#st-2---do-not-use-classic-storage-account)                                                                                                       |    Governance     |  High  | Preview |         Yes          |
| [ST-3 - Ensure Performance tier is set as per workload](#st-3---ensure-performance-tier-is-set-as-per-workload)                                                                               | System Efficiency | Medium | Preview |         Yes          |
| [ST-4 - Choose right blob type for workload](#st-4---choose-right-blob-type-for-workload)                                                                                                     | System Efficiency | Medium | Preview |          No          |
| [ST-5 - Enable soft delete for recovery of data](#st-5---enable-soft-delete-for-recovery-of-data)                                                                                             | Disaster Recovery | Medium | Preview |          No          |
| [ST-6 - Enable version for accidental modification and keep the number of versions below 1000](#st-6---enable-version-for-accidental-modification-and-keep-the-number-of-versions-below-1000) | Disaster Recovery | Medium | Preview |          No          |
| [ST-7 - Enable point and time restore for containers for recovery](#st-7---enable-point-and-time-restore-for-containers-for-recovery)                                                         | Disaster Recovery |  Low   | Preview |          No          |
| [ST-8 - Configure Diagnostic Settings for all storage accounts](#st-8---configure-diagnostic-settings-for-all-storage-accounts)                                                               |    Monitoring     |  Low   | Preview |          No          |
| [ST-9 - Upgrade legacy storage accounts to v2 storage accounts](#st-9---upgrade-legacy-storage-accounts-to-v2-storage-accounts)                                                               | System Efficiency | Medium | Preview |         Yes          |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ST-1 - ストレージ アカウントの構成がゾーン冗長以上であることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

Azure Storage アカウント内のデータは、常にプライマリ リージョンで 3 回レプリケートされます。Azure Storage には、プライマリ リージョンまたはペア リージョンでデータをレプリケートする方法に関する他のオプションが用意されています。

- LRS は、1 つの物理的な場所でデータを 3 回同期的にレプリケートします。レプリケーションは最もコストがかかりませんが、高可用性と持続性を備えたアプリにはお勧めしません。LRS は イレブンナイン の耐久性を提供します。
- ZRS は、プライマリ リージョンの 3 つの可用性ゾーン間でデータを同期的にコピーします。ZRS は、ゾーン間で高可用性を必要とするアプリに推奨されます。ZRS は トゥエルブ ナイン の耐久性を提供します。
- GRS は、追加の 3 つのコピーをセカンダリ リージョンにレプリケートし、シックスティーン ナイン の可用性を提供します。
- GZRS は、geo レプリケーション全体で高可用性と冗長性の両方を提供します。1年間で シックスティーン ナイン の耐久性を提供します。

**Resources**

- [Azure Storage redundancy](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-redundancy)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-1/st-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-2 - クラシック ストレージ アカウントを使用しないようにします

**Category: Governance**

**Impact: High**

**Guidance**

Azure クラシック ストレージ アカウントは、2024 年 8 月 31 日に廃止されます。そのため、すべてのワークロードをクラシック ストレージ アカウントから Azure Resource Manager ストレージ アカウントに移行します。

**Resources**

- [Azure classic storage accounts retirement announcement](https://azure.microsoft.com/updates/classic-azure-storage-accounts-will-be-retired-on-31-august-2024/)
- [Migrate your classic storage accounts to Azure Resource Manager](https://learn.microsoft.com/ja-jp/azure/storage/common/classic-account-migration-overview)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-2/st-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-3 - パフォーマンス レベルがワークロードごとに設定されていることを確認します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

Standard ストレージ、ブロック BLOB、追加 BLOB、ファイル共有、ページ BLOB に適切なストレージ パフォーマンス レベルを使用することを検討してください。各ワークロード シナリオには適切なパフォーマンス レベルが必要であり、トランザクションの種類と BLOB の種類/ファイルの種類に基づいて適切なパフォーマンス レベルを選択することが重要です。そうしないと、パフォーマンスのボトルネックが発生します。

**Resources**

- [Performance Tier](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-account-overview#performance-tiers )

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-3/st-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-4 - ワークロードに適した BLOB の種類を選択します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

ストレージ サービスには、ブロック BLOB、追加 BLOB、ページ BLOB の 3 種類の BLOB が用意されています。BLOB の種類は、BLOB を作成するときに指定します。

ブロック BLOB は、大量のデータを効率的にアップロードするために最適化されています。ブロック BLOB はブロックで構成され、各ブロックはブロック ID で識別されます。ブロック BLOB には、最大 50,000 個のブロックを含めることができます。

追加 BLOB はブロックで構成され、追加操作用に最適化されています。追加 BLOB を変更すると、ブロックは BLOB の末尾にのみ追加されます。既存のブロックの更新や削除はサポートされていません。ブロック BLOB とは異なり、追加 BLOB ではブロック ID は公開されません。

ページ BLOB は、ランダムな読み取りおよび書き込み操作用に最適化された 512 バイトのページのコレクションです。ページ BLOB を作成するには、ページ BLOB を初期化し、ページ BLOB が拡張する最大サイズを指定します。ページ BLOB の内容を追加または更新するには、オフセットと範囲の両方が 512 バイトのページ境界に揃えられるように指定して、ページを書き込みます。

**Resources**

- [Understanding block blobs, append blobs, and page blobs](https://learn.microsoft.com/ja-jp/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)
- [Scalability and performance targets for Blob storage](https://learn.microsoft.com/ja-jp/azure/storage/blobs/scalability-targets)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-4/st-4.kql" >}} {{< /code >}}

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

{{< code lang="powershell" file="code/st-5/st-5.ps1" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-6 - 偶発的な変更に対してバージョン管理を有効にし、バージョンの数を 1000 未満に保ちます

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

誤って変更または削除したデータを回復するには、バージョン管理を有効にします。
BLOB あたりのバージョン数が多いと、BLOB の一覧表示操作の待機時間が長くなる可能性があります。Microsoft では、BLOB あたり 1,000 未満のバージョンを維持することをお勧めします。ライフサイクル管理を使用して、古いバージョンを自動的に削除できます。

**Resources**

- [Blob versioning](https://learn.microsoft.com/ja-jp/azure/storage/blobs/versioning-overview )

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="powershell" file="code/st-6/st-6.ps1" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-7 - リカバリ用コンテナのポイントアンドタイムリストアを有効にします

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

ポイントインタイム リストアを使用すると、ブロック BLOB の 1 つ以上のセットを以前の状態に復元できます。
ポイント アンド タイム リストアでは、Standard パフォーマンス レベルで汎用 v2 アカウントがサポートされます。データを保護するためのメカニズムです。

**Resources**

- [Restore overview](https://learn.microsoft.com/ja-jp/azure/storage/blobs/point-in-time-restore-manage?tabs=portal)

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="powershell" file="code/st-7/st-7.ps1" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-8 - すべてのストレージ アカウントの診断設定を構成します

**Category: Monitoring**

**Impact: Low**

**Guidance**

診断設定を有効にすると、診断情報をキャプチャして表示し、障害のトラブルシューティングを行うことができます。

**Resources**

- [Diagnostic Setting for Storage Account](https://learn.microsoft.com/ja-jp/azure/storage/blobs/monitor-blob-storage)

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="powershell" file="code/st-8/st-8.ps1" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ST-9 - レガシ ストレージ アカウントを v2 ストレージ アカウントにアップグレードします

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

汎用 v2 ストレージ アカウントは、最新の Azure Storage 機能をサポートし、汎用 v1 アカウントと Blob ストレージ アカウントのすべての機能が組み込まれています。汎用 v2 アカウントは、ほとんどのストレージ シナリオに推奨されます。汎用 v2 アカウントは、Azure Storage のギガバイトあたりの容量価格が最も低く、業界で競争力のあるトランザクション価格を提供します。汎用 v2 アカウントでは、ホットまたはクールの既定のアカウント アクセス層と、ホット、クール、またはアーカイブ間の BLOB レベルの階層化がサポートされています。

汎用 v1 または Blob ストレージ アカウントから汎用 v2 ストレージ アカウントへのアップグレードは簡単です。汎用 v2 ストレージ アカウントへのアップグレードに関連するダウンタイムやデータ損失のリスクはありません。汎用 v1 または Blob Storage アカウントから汎用 v2 へのアップグレードは永続的であり、元に戻すことはできません。

**Resources**

- [Legacy storage account types](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-account-overview#legacy-storage-account-types)
- [Upgrade to a general-purpose v2 storage account](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-account-upgrade)

**Script**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/st-9/st-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
