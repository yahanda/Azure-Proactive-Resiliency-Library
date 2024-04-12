+++
title = "Batch Accounts"
description = "Best practices and resiliency recommendations for Batch Accounts and associated resources and settings."
date = "1/12/24"
author = "lapate"
msAuthor = "lapate"
draft = false
+++

The presented resiliency recommendations in this guidance include Batch Accounts (Azure High Performance Computing) and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Impact | Design Area | State | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------|:------:|:------------:|:-------:|:-------------------:|
| [BA-1 - Monitor Batch account quota](#ba-1---monitor-batch-account-quota) | Medium | Monitoring | Preview | No |
| [BA-3 - Create an Azure Batch pool across Availability Zones](#ba-3---create-an-azure-batch-pool-across-availability-zones) | High | Availability | Preview | No |

{{< /table >}}

{{< alert style="info" >}}
Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})
{{< /alert >}}

## Recommendations Details

### BA-1 - Batch アカウントのクォータを監視します

**Category: Monitoring**

**Impact: Medium**

**Recommendation/Guidance**

リージョン間のディザスター リカバリーとビジネス継続性を有効にするには、すべてのユーザー サブスクリプション Batch アカウントに適切なクォータが設定されていることを確認します。これにより、必要な数のコアが前もって割り当てられます。十分なコア容量が割り当てられていないと、ジョブの実行が中断され、「クォータに達しました」という操作エラーが発生します。

各リージョンで必要なすべてのサービス (Batch アカウントやストレージ アカウントなど) を事前に作成します。多くの場合、アカウントの作成には料金はかからず、アカウントが使用されたとき、またはデータが保存されたときにのみ料金が発生します。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/reliability/reliability-batch#cross-region-disaster-recovery-and-business-continuity)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ba-1/ba-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### BA-3 - 複数の Availability Zones にまたがる Azure Batch プールを作成します

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

仮想マシン構成を使用して Azure Batch プールを作成する場合は、複数の Availability Zones にまたがって Batch プールをプロビジョニングすることを選択できます。このゾーン ポリシーを使用してプールを作成すると、Azure データセンター レベルの障害から Batch コンピューティング ノードを保護するのに役立ちます。
たとえば、ゾーン ポリシーを使用して、3 つの可用性ゾーンをサポートする Azure リージョンにプールを作成できます。1 つの可用性ゾーンの Azure データセンターでインフラストラクチャに障害が発生した場合でも、Batch プールには他の 2 つの可用性ゾーンに正常なノードが引き続き存在するため、プールは引き続きタスクのスケジュール設定に使用できます。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/batch/create-pool-availability-zones)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ba-3/ba-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
