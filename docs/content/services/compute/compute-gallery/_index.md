+++
title = "Compute Gallery"
description = "Best practices and resiliency recommendations for Compute gallery and associated resources."
date = "10/12/23"
author = "poven"
msAuthor = "poven"
draft = false
+++

The presented resiliency recommendations in this guidance include Compute Gallery and dependent resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                      |   Category   | Impact |  State   | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------:|:------:|:--------:|:-------------------:|
| [CG-1 - A minimum of three replicas should be kept for production image versions](#cg-1---a-minimum-of-three-replicas-should-be-kept-for-production-image-versions) | Availability | Medium | Verified |         Yes         |
| [CG-2 - Zone redundant storage should be used for image versions](#cg-2---zone-redundant-storage-should-be-used-for-image-versions)                                 | Availability | Medium | Verified |         Yes         |
| [CG-3 - Consider creating TrustedLaunchSupported images where possible](#cg-3---consider-creating-trustedlaunchsupported-images-where-possible)        | Availability |  Low   | Verified |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### CG-1 - 運用イメージ・バージョン用に少なくとも3つのレプリカを保持する必要があります

**Category: Availability**

**Impact: Medium**

**Guidance**

運用イメージ用に少なくとも 3 つのレプリカを保持します。 複数 VM デプロイのシナリオでは、VM デプロイを異なるレプリカに分散できるため、1 つのレプリカの過負荷によってインスタンス作成処理が調整される可能性が低くなります。同時に作成する 20 台の VM ごとに、1 つのレプリカを保持することをお勧めします。たとえば、1,000 個の VM を同時に作成する場合は、50 個のレプリカを保持する必要があります (リージョンごとに最大 50 個のレプリカを持つことができます)。レプリカ数を更新するには、ギャラリー -> イメージ定義 -> イメージ バージョン -> レプリケーションの更新 に移動してください。

**Resources**

- [Compute Gallery best practices](https://learn.microsoft.com/ja-jp/azure/virtual-machines/azure-compute-gallery#best-practices)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cg-1/cg-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CG-2 - イメージ バージョンにゾーン冗長ストレージを使用する必要があります

**Category: Availability**

**Impact: Medium**

**Guidance**

高可用性のために、使用可能な場合は常に ZRS を使用します。ZRS は、イメージまたは VM アプリケーションのバージョンを作成するときに、 レプリケーション タブで構成できます。Azure Zone 冗長ストレージ (ZRS) は、リージョン内の可用性ゾーンの障害に対する回復性を提供します。Azure Compute Gallery の一般提供開始に伴い、[Availability Zones のあるリージョン](https://learn.microsoft.com/ja-jp/azure/availability-zones/az-overview#azure-regions-with-availability-zones) の ZRS アカウントにイメージを格納することを選択できます。
また、対象地域ごとにアカウントの種類を選択することもできます。既定のストレージ アカウントの種類は Standard_LRS ですが、Availability Zones があるリージョンでは Standard_ZRS を選択することをお勧めします。

**Resources**

- [Compute Gallery best practices](https://learn.microsoft.com/ja-jp/azure/virtual-machines/azure-compute-gallery#best-practices)
- [Zone-redundant storage](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-redundancy#zone-redundant-storage)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cg-2/cg-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CG-3 - 可能な場合は、TrustedLaunchSupported イメージの作成を検討してください

**Category: Access & Security**

**Impact: Low**

**Guidance**

セキュア ブート、vTPM、トラステッド起動 VM、大容量ブート ボリュームなどの機能を利用するために、トラステッド起動でサポートされているイメージを作成することをお勧めします。トラステッド起動でサポートされるイメージは、デフォルトで Gen 2 イメージです。仮想マシンの作成後に仮想マシンの世代を変更することはできません。したがって、最初に[考慮事項](https://learn.microsoft.com/ja-jp/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v#which-guest-operating-systems-are-supported)を確認することをお勧めします。

**Resources**

- [Compute Gallery best practices](https://learn.microsoft.com/ja-jp/azure/virtual-machines/azure-compute-gallery#best-practices)
- [Generation 1 vs Generation 2 in Hyper-V](https://learn.microsoft.com/ja-jp/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
- [Images in Compute gallery](https://learn.microsoft.com/ja-jp/azure/virtual-machines/shared-image-galleries?tabs=azure-cli)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cg-3/cg-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
