+++
title = "SAP on Azure"
description = "Best practices and resiliency recommendations for Azure Sap Solution and associated resources and settings."
date = "2/13/24"
author = "humblejay"
msAuthor = "kupole"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure SAP Solution and associated resources and settings.

Refer to -
- Azure Center for SAP Solutions
- Opensource Quality Checks
- Openssource Inventory Checks

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |                                Category                                 |     Impact      |      State       | ARG Query Available |
|:--------------------------------------------------|:-----------------------------------------------------------------------:|:---------------:|:----------------:|:-------------------:|
| [SAP-1 - Ensure that each SAP production system is designed for high availability across availability zones.](#sap-1---ensure-that-each-sap-production-system-is-designed-for-high-availability-across-availability-zones) | Availability | High | Verified | No |
| [SAP-2 - Run SAP application servers on two or more VMs using VMSS Flex.](#sap-2---run-sap-application-servers-on-two-or-more-vms-using-vmss-flex) | Availability | High | Verified | Yes |
| [SAP-9 - If using single-instance VMs, all OS and data disks must be Premium SSD or Ultra Disk.](#sap-9---if-using-single-instance-vms-all-os-and-data-disks-must-be-premium-ssd-or-ultra-disk) | Availability | High | Verified |  Yes  |
| [SAP-14 - Ensure that each database replicates changes synchronously (SYNC mode) to a stand-by node.](#sap-14---ensure-that-the-data-is-replicated-synchronously-sync-mode-between-the-primary-and-secondary-database-hosting-vm-nodes) | Availability | High | Verified |  No  |
| [SAP-15 - Ensure that SAP shared file systems are designed for high availability and when possible using availability zones.](#sap-15---ensure-that-sap-shared-file-systems-are-designed-for-high-availability-and-when-possible-using-availability-zones) | Availability | High | Verified |  No  |
| [SAP-16 - Test high availability solutions thoroughly to ensure fail overs work as expected.](#sap-16---test-high-availability-solutions-thoroughly-to-ensure-fail-overs-work-as-expected) | Availability | High | Verified | No |
| [SAP-18 - Remove unwanted location constraints from Linux Pacemaker clusters.](#sap-18---remove-unwanted-location-constraints-from-linux-pacemaker-clusters) | Availability | High | Verified | No |
| [SAP-26 - Secure compute resource capacity for critical VM roles in DR region.](#sap-26---secure-compute-resource-capacity-for-critical-vm-roles-in-dr-region) | Disaster Recovery | Medium | Verified | No |
| [SAP-27 - Ensure that the production databases are replicated (ASYNC) to DR location using the database vendor's replication technology.](#sap-27---ensure-that-the-production-databases-are-replicated-async-to-dr-location-using-the-database-vendors-replication-technology) | Disaster Recovery | High | Verified | No |
| [SAP-28 - SAP components are backed up to DR location using an appropriate backup tool or ASR.](#sap-28---sap-components-are-backed-up-to-dr-location-using-an-appropriate-backup-tool-or-asr) | Disaster Recovery | High | Verified | No |
| [SAP-29 - SAP shared files systems are replicated or backed up to DR location.](#sap-29---sap-shared-files-systems-are-replicated-or-backed-up-to-dr-location) | Disaster Recovery | High | Verified | No |
| [SAP-32 - Automate DR infrastructure build or pre-deploy DR resources.](#sap-32---automate-dr-infrastructure-build-or-pre-deploy-dr-resources) | Disaster Recovery | Medium | Verified | No |
| [SAP-33 - Document and test DR procedure ensure it meets RPO and RTO targets.](#sap-33---document-and-test-dr-procedure-ensure-it-meets-rpo-and-rto-targets) | Disaster Recovery | Medium | Verified | No |
| [SAP-34 - Ensure there is a robust monitoring and alerting solution in place for the entire DR solution.](#sap-34---ensure-there-is-a-robust-monitoring-and-alerting-solution-in-place-for-the-entire-dr-solution) | Disaster Recovery | Medium | Verified | No |
| [SAP-36 - Configure scheduled events notification](#sap-36---configure-scheduled-events-notification) | Monitor | High | Verified | No |
| [SAP-42 - ASCS-Pacemaker (Central Server Instance) Ensure Pacemaker cluster has been setup for SAP ASCS high availability.](#sap-42---ascs-pacemaker-central-server-instance-ensure-pacemaker-cluster-has-been-setup-for-sap-ascs-high-availability) | Availability | High | Verified | No |
| [SAP-45 - ASCS-LB (Central Server Instance) Ensure the load balancer is configured correctly for SAP ASCS High availability.](#sap-45---ascs-lb-central-server-instance-ensure-the-load-balancer-is-configured-correctly-for-sap-ascs-high-availability) | Availability | High | Verified | No |
| [SAP-46 - DBHANA-Pacemaker (Database Instance) Ensure the Pacemaker cluster has been setup for SAP HANA DB high availability.](#sap-46---dbhana-pacemaker-database-instance-ensure-the-pacemaker-cluster-has-been-setup-for-sap-hana-db-high-availability) | Availability | High | Verified | No |
| [SAP-49 - DBHANA-LB (Database Instance) Ensure the load balancer is configured correctly for SAP HANA DB High availability.](#sap-49---dbhana-lb-database-instance-ensure-the-load-balancer-is-configured-correctly-for-sap-hana-db-high-availability) | Availability | High | Verified | No |


{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### SAP-1 - 各 SAP 運用システムが、可用性ゾーン間で高可用性を実現するように設計されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

Azure Availability Zones は、各 Azure リージョン内の物理的に分離された場所であり、ローカルの障害に耐えられます。アベイラビリティーゾーンを使用して、データセンターの予期せぬ障害からアプリケーションとデータを保護します。複数の可用性ゾーンを使用して、各 SAP 運用システムの各単一障害点が高可用性で保護されるようにします。リージョン内の異なるゾーンにデプロイできない場合は、SAP ワークロードの高可用性デプロイ オプションに関する Microsoft のガイダンスを参照してください。

**Resources**

- [SAP ACSS Quality Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Inventory Checks](https://aka.ms/ACESInventoryCheckSAP)
- [OpenSource Quality Checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)
- [Move Regional SAP HA to Zonal](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/Move-VM-from-AvSet-to-AvZone/Move-Regional-SAP-HA-To-Zonal-SAP-HA-WhitePaper)
- [High Availability Deployment Options for SAP](https://learn.microsoft.com/ja-jp/azure/sap/workloads/sap-high-availability-architecture-scenarios#high-availability-deployment-options-for-sap-workload)


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-1/sap-1.kql">}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-2 - VMSS Flex を使用して 2 つ以上の VM で SAP アプリケーション サーバーを実行します

**Category: Availability**

**Impact: High**

**Guidance**

柔軟なオーケストレーションで Virtual Machines スケール セット (VMSS) を使用して、指定したゾーン間および各ゾーン内で仮想マシンを分散し、ゾーン内の異なる障害ドメインにベスト エフォート ベースで VM を分散します。適切なモードと正しい設定を使用して、SAP ワークロードに関する Microsoft の推奨事項に従って VMSS Flex を構成します。現在、VMSS Flex for SAP アプリケーション サーバーを使用しておらず、障害ドメインと更新ドメインの分散で可用性セットも使用していない場合は、VMSS Flex アーキテクチャへの移行を検討して、SAP デプロイの回復性体制を改善する必要があります。以下のリンクにある次のブログ記事では、可用性セットまたは可用性ゾーンにデプロイされている既存の SAP ワークロードを、FD=1 デプロイ オプションを使用してフレキシブル スケール セットに移行するプロセスの詳細について概説しています。


**Resources**

- [OpenSource Inventory Checks](https://aka.ms/ACESInventoryCheckSAP)
- [Virtual machine Scale Set SAP Deployment Guide](https://learn.microsoft.com/ja-jp/azure/sap/workloads/virtual-machine-scale-set-sap-deployment-guide)
- [Considerations for Flexible VM Scale Sets for SAP](https://learn.microsoft.com/ja-jp/azure/sap/workloads/virtual-machine-scale-set-sap-deployment-guide?tabs=scaleset-cli#important-consideration-of-flexible-virtual-machine-scale-sets-for-sap-workload)
- [Migrate existing SAP system VMs to VMSS Flex](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/how-to-easily-migrate-an-existing-sap-system-vms-to-flexible/ba-p/3833548)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="../../compute/virtual-machines/code/vm-1/vm-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-9 - 単一インスタンス VM を使用する場合、すべての OS ディスクとデータ ディスクが Premium SSD または Ultra Disk である必要があります

**Category: Availability**

**Impact: High**

**Guidance**

For single-instance VMs, both OS and data disks must be either Premium SSD or Ultra Disk to achieve the single-instance SLA of 99.9% availability.

**Resources**

- [SAP ACSS Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Inventory Checks](https://aka.ms/ACESInventoryCheckSAP)
- [OpenSource Quality Checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)
- [VM SLA](https://www.azure.cn/ja-jp/support/sla/virtual-machines/)
- [SAP Storage Planning Guide](https://learn.microsoft.com/ja-jp/azure/sap/workloads/planning-guide-storage)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="../../compute/virtual-machines/code/vm-24/vm-24.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-14 - VM ノードをホストしているプライマリ データベースとセカンダリ データベース間でデータが同期的にレプリケートされる (SYNC モード) ことを確認します

**Category: Availability**

**Impact: High**

**Guidance**

データベースの高可用性は、データベース・ネイティブ・レプリケーション・テクノロジーを使用して実装する必要があり、データは同期的に、つまりプライマリ・データベースからスタンバイ・ノードに同期モードで複製する必要があります。

**Resources**

- [SAP ACSS Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality Checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-14/sap-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-15 - SAP 共有ファイル システムが高可用性を実現するように設計されており、可能な場合は可用性ゾーンを使用していることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

/sapmnt、/usr/sap/trans などの SAP 共有ファイルシステムのインターフェースは、高可用性にする必要があります。

Azure ファイル共有の場合は、ZRS (ゾーン冗長ストレージ) を使用することをお勧めします。
Azure NetApp Files の場合は、ボリュームにゾーン レプリケーションを使用することをお勧めします。

他の Azure サービスに対する個々のチェックの結果を確認して、SAP 共有ファイル システムがゾーン障害から保護するように設計されていることを確認する必要があります (ST-1、ANF-1、ANF-6)

**Resources**

- [OpenSource Inventory Checks](https://aka.ms/ACESInventoryCheckSAP)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-15/sap-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-16 - 高可用性ソリューションを徹底的にテストして、フェールオーバーが期待どおりに機能することを確認します

**Category: Availability**

**Impact: High**

**Guidance**

すべての高可用性ソリューションを徹底的にテストします (Linux VM でのカーネル パニックとフェールバックを含む)。テストにゾーン障害シナリオを含めると、データベース、セントラル サービス、アプリケーション サーバー、共有ファイル システムなど、SAP ソリューションの各レイヤーがゾーン冗長性に対して正しく構成され、ソリューションが RPO = 0 を満たし、アプリケーションが自動的にフェールオーバーして RTO を満たすことがテストで確認されます。
フェールバックは、自動または手動のいずれかです。

**Resources**

- [Test Cases](https://learn.microsoft.com/ja-jp/azure/sap/workloads/sap-hana-high-availability?tabs=lb-portal#test-the-cluster-setup)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-16/sap-16.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-18 - Linux Pacemaker クラスターから不要な場所の制約を削除します

**Category: Availability**

**Impact: High**

**Guidance**

Linux Pacemaker クラスターで migrate コマンドを実行すると、システムは、指定されたノードにリソースを移動することを目的とした一時的な "優先" 場所の制約を生成します。この制約は、クラスターの構成を永続的に変更することなく、リソースのターゲットノードに一時的に優先順位を付けます。

計画メンテナンスおよびフェイルオーバー・テスト中に、保守または管理タスク中の一時的なリソース再配置にmigrateコマンドを利用して、中断を最小限に抑えることができます。この制約は永続的ではなく、再起動やクラスタのリセット後も存続しません。これは、短期的な調整用に設計されています。

リソースの移行を必要とする計画されたタスクが完了したら、一時的な制約を手動で削除して、クラスターの元のリソース管理ポリシーに戻します。
このアプローチにより、クラスター内のリソース移動を制御できるため、クラスターの構成の整合性と効率を維持しながらメンテナンスが容易になります。

**Resources**


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-18/sap-18.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-26 - DR リージョン内の重要な VM ロールのコンピューティング リソース容量を確保します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

DR リージョン内の重要な VM ロールのコンピューティング リソースの可用性を確保するには、ウォーム スタンバイ アプローチまたは Azure のオンデマンド容量予約を利用して容量を確保することを検討してください。

ウォーム スタンバイでは、DR リージョン内の VM を実行したままにします。一方、オンデマンド容量予約 では、VM を実行せずにコンピューティング容量が予約されるため、必要なときに起動できます。DR VM が不要な場合は、予約容量を安全に使用して他のワークロードを実行し、他のお客様に容量を奪われるリスクを負わせることはありません。この戦略により、災害発生時に重要なワークロードのリソースの可用性が保証され、コストと準備のバランスが取れます。

**Resources**

- [Capacity Reservation](https://learn.microsoft.com/ja-jp/azure/virtual-machines/capacity-reservation-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-26/sap-26.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-27 - データベース ベンダーのレプリケーション テクノロジを使用して、運用データベースが DR の場所にレプリケート (ASYNC) されていることを確認します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

データベース・ベンダーの非同期レプリケーション・テクノロジーを使用して、本番データベースをDRロケーションにレプリケーションすることは、データの可用性とビジネス継続性を確保するための重要な戦略です。

**Resources**

- [SAP Disaster Recovery Guide](https://learn.microsoft.com/ja-jp/azure/sap/workloads/disaster-recovery-sap-guide?tabs=windows)


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-27/sap-27.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-28 - SAP コンポーネントは、適切なバックアップ ツールまたは ASR を使用して DR の場所にバックアップされます

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

(A)SCS、アプリケーションサーバー、WebDispatcherなどのSAPコンポーネントは、適切なバックアップツールまたはASRを使用してDRロケーションにバックアップされます。

**Resources**

- [SAP ACSS Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Inventory Checks](https://aka.ms/ACESInventoryCheckSAP)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-28/sap-28.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-29 - SAP共有ファイル・システムは、DRの場所に複製またはバックアップされます

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

/sapmnt、/usr/trans、/interfaces などの重要な SAP 共有ファイルシステムが、災害復旧の目的で複製またはバックアップされていることを確認します。

**Resources**

- [DR Guidance](https://learn.microsoft.com/ja-jp/azure/sap/workloads/disaster-recovery-sap-guide?tabs=windows)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-29/sap-29.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-32 - DR インフラストラクチャの構築または DR リソースの事前デプロイを自動化します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

DRインフラストラクチャの構築(またはDRリソースの事前デプロイ)とSAPサービスの復旧を可能な限り自動化します。

**Resources**


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-32/sap-32.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-33 - DR手順を文書化してテストし、RPOとRTOの目標を満たしていることを確認します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

SAP アーキテクチャの各レイヤー (データベース、セントラル サービス、アプリケーション サーバー、共有ファイル システム) の DR 手順の詳細なドキュメントを作成します。このドキュメントには、構成の詳細、フェールオーバー メカニズム、および段階的な回復手順が含まれている必要があります。

リージョンの停止を含む、さまざまな障害シナリオをテストします。テストでは、DR 戦略が堅牢であり、RPO と RTO の目標を満たし、SAP アーキテクチャのすべてのレイヤーでシームレスなフェールオーバーを提供することを確認する必要があります。

これにより、地域の障害に耐え、事業継続性を確保できる包括的で回復力のあるDR戦略が保証されます。

**Resources**


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-33/sap-33.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-34 - DR ソリューション全体に対して堅牢な監視およびアラートソリューションが導入されていることを確認します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

Azure でホストされている SAP ソリューションの場合、SAP アーキテクチャの各レイヤーの DR を包括的にカバーする堅牢な監視およびアラート ソリューションを実装することが不可欠です。SAP システムは、さまざまなテクノロジと Azure リソースを使用して複数のレイヤーにまたがり、それぞれが異なる DR レプリケーション メカニズムを持つ可能性があるため、適切な監視戦略が不可欠です。さまざまなレイヤーには、データベース、セントラル サービス、アプリケーション、および共有ファイル システムが含まれます。

**Resources**


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-34/sap-34.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-36 - スケジュールされたイベントの通知を構成します

**Category: Monitor**

**Impact: High**

**Guidance**

スケジュールされたイベントは、今後のメンテナンス イベント (再起動など) に関するプロアクティブな通知を提供する Azure Metadata Services であり、アプリケーションがそれらに備え、中断を制限できるようにします。すべての重要な Azure VM に対してスケジュールされたイベントを構成する必要があります。
リソース エージェント azure-events-az は、Pacemaker クラスターと統合することもできます。

Azure VM の高可用性とサービス継続性を確保するには、Pacemaker クラスター内で azure-events-az リソース エージェントを構成する必要があります。このエージェントは、スケジュールされた Azure メンテナンス イベントを監視し、ノードの正常なシャットダウンのためにリソースを事前に再配置できます。再起動や再デプロイなどの特定のイベントの種類を監視するようにエージェントを構成し、詳細な診断のために詳細ログを有効にします。

また、スケジュールされたイベントへの対応方法に関する手順を定義することも重要です。

**Resources**

- [VM Scheduled Events](https://learn.microsoft.com/ja-jp/azure/virtual-machines/linux/scheduled-events)
- [Configure Pacemaker for Azure Scheduled Events](https://learn.microsoft.com/ja-jp/azure/sap/workloads/high-availability-guide-suse-pacemaker?tabs=msi#configure-pacemaker-for-azure-scheduled-events)


**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-36/sap-36.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-42 - ASCS-Pacemaker (セントラル サーバー インスタンス) Pacemaker クラスターが SAP ASCS 高可用性用にセットアップされていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

ASCS-Pacemaker (セントラル サーバー インスタンス) の場合は、Pacemaker クラスター構成パラメーターが SAP ASCS の高可用性用に正しく設定されていることを確認します。

**Resources**

- [SAP ACSS Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality Checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)
- [ASCS-Pacemaker - Central Server Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-42/sap-42.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-45 - ASCS-LB (セントラル サーバー インスタンス) ロード バランサーが SAP ASCS 高可用性用に正しく構成されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

ASCS-LB (セントラル サーバー インスタンス) の場合は、ロード バランサーが SAP ASCS 高可用性用に正しく構成されていることを確認します。

**Resources**

- [SAP ACSS Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality Checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)
- [ASCS-LB - Central Server Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-45/sap-45.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-46 - DBHANA-Pacemaker (データベース インスタンス) Pacemaker クラスターが SAP HANA DB の高可用性用にセットアップされていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

DBHANA-Pacemaker (データベース インスタンス) の場合は、Pacemaker クラスター構成パラメーターが SAP HANA DB の高可用性用に正しく設定されていることを確認します。

**Resources**

- [SAP ACSS Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality Checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)
- [DBHANA-Pacemaker - Database Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-46/sap-46.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-49 - DBHANA-LB (データベース インスタンス) ロードバランサーが SAP HANA DB 高可用性用に正しく構成されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

DBHANA-LB (データベース インスタンス) の場合は、ロード バランサーが SAP HANA DB の高可用性用に正しく構成されていることを確認します。

**Resources**

- [SAP ACSS Insights](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality Checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)
- [DBHANA-LB- Database Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-49/sap-49.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
