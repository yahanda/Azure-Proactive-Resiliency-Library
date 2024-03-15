+++
title = "Virtual Machine Scale Sets"
description = "Best practices and resiliency recommendations for Virtual Machine Scale Sets and associated resources."
date = "5/23/23"
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented resiliency recommendations in this guidance include Virtual Machine Scale Sets, and dependent resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [VMSS-1 - Deploy VMSS with Flex orchestration mode instead of Uniform](#vmss-1---deploy-vmss-with-flex-orchestration-mode-instead-of-uniform) | System Efficiency | Medium | Preview | Yes |
| [VMSS-2 - Enable VMSS application health monitoring](#vmss-2---enable-vmss-application-health-monitoring) | Monitoring | Medium | Preview | Yes |
| [VMSS-3 - Enable Automatic Repair policy](#vmss-3---enable-automatic-repair-policy) | Automation | High | Preview | Yes |
| [VMSS-4 - Configure VMSS autoscale to custom and configure the scaling metrics](#vmss-4---configure-vmss-autoscale-to-custom-and-configure-the-scaling-metrics) | System Efficiency | High | Preview | Yes |
| [VMSS-5 - Enable Predictive Autoscale and configure at least for Forecast Only](#vmss-5---enable-predictive-autoscale-and-configure-at-least-for-forecast-only) | System Efficiency | Low | Preview | Yes |
| [VMSS-6 - Disable Force strictly even balance across zones to avoid scale in and out fail attempts](#vmss-6---disable-force-strictly-even-balance-across-zones-to-avoid-scale-in-and-out-fail-attempts) | Availability | High | Preview | Yes |
| [VMSS-7 - Configure Allocation Policy Spreading algorithm to Max Spreading](#vmss-7---configure-allocation-policy-spreading-algorithm-to-max-spreading) | System Efficiency | Medium | Preview | Yes |
| [VMSS-8 - Deploy VMSS across availability zones with VMSS Flex](#vmss-8---deploy-vmss-across-availability-zones-with-vmss-flex) | Availability | High | Preview | Yes |
| [VMSS-9 - Set Patch orchestration options to Azure-orchestrated](#vmss-9---set-patch-orchestration-options-to-azure-orchestrated) | Automation | Low | Preview | Yes |
| [VMSS-10 - Upgrade VMSS Image versions scheduled to be deprecated or already retired](#vmss-10---upgrade-vmss-image-versions-scheduled-to-be-deprecated-or-already-retired) | Governance | High | Preview | Yes |
| [VMSS-11 - Production VMSS instances should be using SSD disks](#vmss-11---production-vmss-instances-should-be-using-ssd-disks) | System Efficiency | High | Preview | Yes |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### VMSS-1 - 均一オーケストレーションではなく Flex オーケストレーション モードで VMSS を展開します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

単一インスタンスの VM でも、フレキシブル オーケストレーション モードを使用してスケール セットにデプロイし、アプリケーションのスケーリングと可用性を将来にわたって保証する必要があります。柔軟なオーケストレーションは、リージョン内または可用性ゾーン内の障害ドメインに VM を分散することで、高可用性の保証 (最大 1,000 VM) を提供します。

**Resources**

- [When to use VMSS instead of VMs](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#when-to-use-scale-sets-instead-of-virtual-machines)
- [Azure Well-Architected Framework review - Virtual Machines and Scale Sets](https://learn.microsoft.com/ja-jp/azure/well-architected/services/compute/virtual-machines/virtual-machines-review)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-1/vmss-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-2 - VMSS アプリケーションの正常性監視を有効にします

**Category: Monitoring**

**Impact: Medium**

**Guidance**

アプリケーションの正常性の監視は、デプロイを管理およびアップグレードするための重要なシグナルです。Azure Virtual Machine Scale Sets は、OS イメージの自動アップグレードや VM ゲストの自動パッチ適用などのローリング アップグレードをサポートしており、個々のインスタンスの正常性の監視に依存してデプロイをアップグレードします。また、Application Health Extension を使用して、スケール セット内の各インスタンスのアプリケーション正常性を監視し、自動インスタンス修復を使用してインスタンスの修復を実行することもできます。

**Resources**

- [Using Application Health extension with Virtual Machine Scale Sets](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension?tabs=rest-api)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-2/vmss-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-3 - 自動修復ポリシーを有効にする

**Category: Automation**

**Impact: High**

**Guidance**

Azure Virtual Machine Scale Sets のインスタンスの自動修復を有効にすると、一連の正常なインスタンスを維持することで、アプリケーションの高可用性を実現できます。Application Health 拡張機能または Load Balancer の正常性プローブで、インスタンスが異常であることが検出される場合があります。自動インスタンス修復では、異常なインスタンスを削除し、それを置き換える新しいインスタンスを作成することで、インスタンスの修復が自動的に実行されます。

猶予期間は ISO 8601 形式で分単位で指定され、プロパティ automaticRepairsPolicy.gracePeriod を使用して設定できます。猶予期間の範囲は 10 分から 90 分で、既定値は 30 分です。

**Resources**

- [Automatic instance repairs for Azure Virtual Machine Scale Sets](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-instance-repairs#requirements-for-using-automatic-instance-repairs)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-3/vmss-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-4 - VMSS Autoscale をカスタムに構成し、スケーリング メトリックを構成します

**Category: System Efficiency**

**Impact: High**

**Recommendation**

メトリックとスケジュールに基づくカスタム自動スケーリングを使用します。

自動スケーリングは、需要の変化に応じてアプリケーションが最適なパフォーマンスを発揮するのに役立つ組み込み機能です。リソースを特定のインスタンス数に手動でスケーリングするか、メトリックのしきい値に基づいてスケーリングするカスタム自動スケール ポリシーを使用してスケーリングするか、指定した時間枠内にスケーリングするインスタンス数をスケジュールするかを選択できます。自動スケーリングにより、需要に基づいてインスタンスを追加および削除することで、リソースのパフォーマンスとコスト効率を高めることができます。

**Resources**

- [Get started with autoscale in Azure](https://learn.microsoft.com/ja-jp/azure/azure-monitor/autoscale/autoscale-get-started?WT.mc_id=Portal-Microsoft_Azure_Monitoring)
- [Overview of autoscale in Azure](https://learn.microsoft.com/ja-jp/azure/azure-monitor/autoscale/autoscale-overview)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-4/vmss-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-5 - 予測自動スケーリングを有効にし、少なくとも予測のみに構成します

**Category: System Efficiency**

**Impact: Low**

**Guidance**

予測自動スケーリングでは、機械学習を使用して、周期的なワークロード パターンで Azure Virtual Machine Scale Sets を管理およびスケーリングします。これにより、過去の CPU 使用率パターンに基づいて、仮想マシン スケール セットに対する全体的な CPU 負荷が予測されます。使用状況の履歴を観察して学習することで、全体的な CPU 負荷を予測します。このプロセスにより、需要を満たす時間内にスケールアウトが確実に行われます。

**Resources**

- [Use predictive autoscale to scale out before load demands in virtual machine scale sets](https://learn.microsoft.com/ja-jp/azure/azure-monitor/autoscale/autoscale-predictive)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-5/vmss-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-6 - スケールインおよびスケールアウトの失敗を回避するために、ゾーン間で均等に分散を強制する設定を無効にします

**Category: Availability**

**Impact: High**

**Guidance**

Microsoft では、VMSS 構成のリージョン内の可用性ゾーン間で VM インスタンスを厳密に均等に分散する設定を無効にすることをお勧めします。つまり、Azure が VM インスタンスをアベイラビリティーゾーン間で不均等に分散できるようにする必要があります。

ゾーン間で厳密に均等にバランスを取る: Azure には、VMSS 内の VM インスタンスをリージョン内の可用性ゾーン間で均等に分散するオプションが用意されています。可用性ゾーンは、独立した電源、冷却装置、ネットワークを備えた Azure リージョン内の物理的に分離されたデータ センターです。この構成により、アプリケーションの可用性とフォールトトレランスが向上します。

スケールインとスケールアウトの失敗の試行: VMSS のコンテキストでは、"スケールイン" とは、需要が減少したときに VM インスタンスの数を減らすことを指し、"スケールアウト" とは、需要が増加したときにインスタンスの数を増やすことを指します。スケーリングは VMSS の重要な機能であり、さまざまなスケーリング ルールとメトリックに基づいて自動的に行うことができます。

Azure VMSS には、回復性を高めるために可用性ゾーン間で VM インスタンスを均等に分散するオプションが用意されていますが、アプリケーションの負荷分散とスケーリングの要件に合わせて、このオプションを無効にすることが理にかなっているシナリオがある場合があります。

**Resources**

- [Use scale-in policies with Azure Virtual Machine Scale Sets](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-scale-in-policy)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-6/vmss-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-7 - 割り当てポリシーの拡散アルゴリズムを最大拡散に構成します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

最大拡散では、スケール セットによって、各ゾーン内のできるだけ多くの障害ドメインに VM が拡散されます。この拡散は、ゾーンごとに 5 つを超えるフォルト ドメインまたはより少ないフォルト ドメインにまたがる可能性があります。静的固定拡散では、スケール セットによって、ゾーンごとに正確に 5 つの障害ドメインに VM が拡散されます。スケール セットが、割り当て要求を満たすためにゾーンごとに 5 つの異なる障害ドメインを見つけられない場合、要求は失敗します。

**Resources**

- [Availability Considerations](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones#availability-considerations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-7/vmss-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-8 - VMSS Flex を使用して可用性ゾーン間で VMSS をデプロイします

**Category: Availability**

**Impact: High**

**Guidance**

VMSS を作成するときは、可用性ゾーンを使用して、データセンターで発生する可能性の低い障害からアプリケーションとデータを保護します。

**Resources**

- [Create a Virtual Machine Scale Set that uses Availability Zones](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones)
- [Update scale set to add availability zones](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones?tabs=cli-1%2Cportal-2#update-scale-set-to-add-availability-zones)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-8/vmss-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-9 - パッチ オーケストレーション オプション を Azure-orchestrated に設定します

**Category: Automation**

**Impact: Low**

**Guidance**

Azure VM の VM ゲストの自動修正プログラム適用を有効にすると、仮想マシンに安全かつ自動的に修正プログラムを適用してセキュリティ コンプライアンスを維持しながら、VM の影響範囲を制限することで、更新管理が容易になります。 以下の KQL では、均一オーケストレーションを使用した仮想マシン スケール セットは返されないことに注意してください。

**Resources**

- [Automatic VM Guest Patching for Azure VMs](https://learn.microsoft.com/ja-jp/azure/virtual-machines/automatic-vm-guest-patching)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-9/vmss-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-10 - 非推奨予定または既に廃止予定の VMSS イメージ バージョンをアップグレードします

**Category: Governance**

**Impact: High**

**Guidance**

イメージの廃止後の中断を回避するために、イメージの現在のバージョンが使用されていることを確認します。これにより、これらのイメージが非推奨になっても、イメージが非推奨になると追加の VM または VMSS をデプロイできなくなるため、影響を受けません。

**Resources**

- [Deprecated Azure Marketplace images](https://learn.microsoft.com/ja-jp/azure/virtual-machines/deprecated-images)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-10/vmss-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VMSS-11 - 運用 VMSS インスタンスでは SSD ディスクを使用する必要があります

**Category: System Efficiency**

**Impact: High**

**Guidance**

運用環境のワークロードには SSD ディスクを使用することをお勧めします。HDD は重要でないリソースや、アクセス頻度の低いリソースにのみ使用する必要があるため、HDD を使用すると、リソースに影響を与える可能性があります。

**Resources**

- [Disk Comparison](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-types#disk-type-comparison)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vmss-11/vmss-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
