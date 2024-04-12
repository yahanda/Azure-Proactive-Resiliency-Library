+++
title = "Virtual Machines"
description = "Best practices and resiliency recommendations for Virtual Machines and associated resources."
date = "3/30/23"
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented resiliency recommendations in this guidance include Virtual Machines and dependent resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------:|:------:|:--------:|:-------------------:|
| [VM-1 - Run production workloads on two or more VMs using VMSS Flex](#vm-1---run-production-workloads-on-two-or-more-vms-using-vmss-flex) | Availability | High | Verified | Yes |
| [VM-2 - Deploy VMs across Availability Zones](#vm-2---deploy-vms-across-availability-zones) | Availability | High | Verified | Yes |
| [VM-3 - Migrate VMs using availability sets to VMSS Flex](#vm-3---migrate-vms-using-availability-sets-to-vmss-flex) | Availability | High | Verified | Yes |
| [VM-4 - Replicate VMs using Azure Site Recovery](#vm-4---replicate-vms-using-azure-site-recovery) | Disaster Recovery | Medium | Verified | Yes |
| [VM-5 - Use Managed Disks for Virtual Machine disks](#vm-5---use-managed-disks-for-vm-disks) | Availability | High | Verified | Yes |
| [VM-6 - Host database data on a data disk](#vm-6---host-database-data-on-a-data-disk) | System Efficiency | Low | Verified | Yes |
| [VM-7 - Enable Backups on your VMs](#vm-7---backup-vms-with-azure-backup-service) | Disaster Recovery | Medium | Verified | Yes |
| [VM-8 - Production VMs should be using SSD disks](#vm-8---production-vms-should-be-using-ssd-disks) | System Efficiency | High | Verified | Yes |
| [VM-9 - There are VMs in Stopped state](#vm-9---review-vms-in-stopped-state) | Governance | Low | Verified | Yes |
| [VM-10 - Accelerated Networking is not enabled](#vm-10---enable-accelerated-networking-accelnet) | System Efficiency | Medium | Verified | Yes |
| [VM-11 - Accelerated Networking is enabled, make sure you update the GuestOS NIC driver every 6 months](#vm-11---when-accelnet-is-enabled-you-must-manually-update-the-guestos-nic-driver) | Governance | Low | Verified | No |
| [VM-12 - VMs should not have a Public IP directly associated](#vm-12---vms-should-not-have-a-public-ip-directly-associated) | Access & Security | Medium | Verified | Yes |
| [VM-13 - VM network interfaces and associated subnets both have a Network Security Group (NSG) associated](#vm-13---vm-network-interfaces-and-associated-subnets-both-have-a-network-security-group-nsg-associated) | Access & Security | Low | Verified | Yes |
| [VM-14 - IP Forwarding should only be enabled for Network Virtual Appliances](#vm-14---ip-forwarding-should-only-be-enabled-for-network-virtual-appliances) | Access & Security | Medium | Verified | Yes |
| [VM-15 - Customer DNS Servers should be configured in the Virtual Network level](#vm-15---customer-dns-servers-should-be-configured-in-the-virtual-network-level) | Networking | Low | Verified | Yes |
| [VM-16 - Shared disks should only be enabled in Clustered servers](#vm-16---shared-disks-should-only-be-enabled-in-clustered-servers) | Storage | Medium | Verified | Yes |
| [VM-17 - The Network access to the VM disk is set to Enable Public access from all networks](#vm-17---network-access-to-the-vm-disk-should-be-set-to-disable-public-access-and-enable-private-access) | Access & Security | Low | Verified | Yes |
| [VM-18 - Virtual Machine is not compliant with Azure Policies](#vm-18---ensure-that-your-vms-are-compliant-with-azure-policies) | Governance | Low | Verified | Yes |
| [VM-19 - Enable advanced encryption options for your managed disks](#vm-19---enable-advanced-encryption-options-for-your-managed-disks) | Access & Security | Medium | Verified | No |
| [VM-20 - Enable Insights to get more visibility into the health and performance of your virtual machine](#vm-20---enable-vm-insights) | Monitoring | Low | Verified | Yes |
| [VM-21 - Configure diagnostic settings for all Azure Virtual Machines](#vm-21---configure-diagnostic-settings-for-all-azure-virtual-machines) | Monitoring | Low | Preview | Yes |
| [VM-22 - Use maintenance configurations for the Virtual Machine](#vm-22---use-maintenance-configurations-for-the-vms) | Governance | High | Verified | Yes |
| [VM-23 - Avoid using A or B-Series VM Sku for production VMs that need the full performance of the CPU continuously](#vm-23---avoid-using-a-or-b-series-vm-sku-for-production-vms-that-need-the-full-performance-of-the-cpu-continuously) | System Efficiency | High | Verified | Yes |
| [VM-24 - Mission Critical Workloads should be using Premium or Ultra Disks](#vm-24---mission-critical-workloads-should-be-using-premium-or-ultra-disks) | System Efficiency | High | Verified | Yes |
| [VM-27 - Use Azure Boost VMs for Maintenance sensitive workload](#vm-27---use-azure-boost-vms-for-maintenance-sensitive-workload) | Availability | Medium | Preview | No |
| [VM-28 - Enable Scheduled Events for Maintenance sensitive workload VMs](#vm-28---enable-scheduled-events-for-maintenance-sensitive-workload-vms) | Availability | Medium | Preview | No |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### VM-1 - VMSS Flex を使用して 2 つ以上の VM で運用ワークロードを実行します

**Category: Availability**

**Impact: High**

**Guidance**

本番環境の VM ワークロードは、複数の VM にデプロイし、VMSS Flex インスタンスにグループ化する必要があります。VMSS Flex は、プラットフォーム全体に VM をインテリジェントに分散し、プラットフォームの障害やプラットフォームの更新がワークロードに与える影響を最小限に抑えます。単一インスタンス VM で実行されているワークロードは、それらのインスタンスが複数の可用性ゾーンに分散している場合でも、VM が相互に関連していることをプラットフォームが認識する方法がないため、同じ保護を受けることはできません。

**Resources**

- [What has changed with Flexible orchestration mode](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-orchestration-modes#what-has-changed-with-flexible-orchestration-mode)
- [Attach or detach a Virtual Machine to or from a Virtual Machine Scale Set](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-attach-detach-vm?branch=main&tabs=portal-1%2Cportal-2%2Cportal-3)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-1/vm-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-2 - 可用性ゾーン間で VM をデプロイします

**Category: Availability**

**Impact: High**

**Guidance**

Azure Availability Zones は、各 Azure リージョン内の物理的に分離された場所であり、ローカルの障害に耐えられます。可用性ゾーンを使用して、データセンターの予期せぬ障害からアプリケーションとデータを保護します。

**Resources**

- [Create virtual machines in an availability zone using the Azure portal](https://learn.microsoft.com/ja-jp/azure/virtual-machines/create-portal-availability-zone?tabs=standard)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-2/vm-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-3 - 可用性セットを使用した VM を VMSS Flex に移行します

**Category: Availability**

**Impact: High**

**Guidance**

可用性セットは近い将来廃止される予定です。ワークロードを VM から VMSS Flex に移行することで、ワークロードを最新化します。VMSS Flex では、次の 2 つの方法のいずれかで VM をデプロイできます。

.ゾーン間
.同じゾーン内ですが、フォルト・ドメイン(FD)と更新ドメイン(UD)を自動的にまたがります。

N 層アプリケーションでは、各アプリケーション層を独自の VMSS Flex に配置することをお勧めします。

**Resources**

- [Resiliency checklist for Virtual Machines](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#virtual-machines)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-3/vm-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-4 - Azure Site Recovery を使用して VM をレプリケートします

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

Site Recovery を使用して Azure VM をレプリケートすると、すべての VM ディスクがターゲット リージョンに非同期的に継続的にレプリケートされます。復旧ポイントは数分ごとに作成されます。これにより、分単位で目標復旧時点 (RPO) が得られます。ディザスター リカバリーの訓練は、運用アプリケーションや進行中のレプリケーションに影響を与えることなく、何度でも実行できます。

**Resources**

- [Resiliency checklist for Virtual Machines](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#virtual-machines)
- [Run a test failover (disaster recovery drill) to Azure](https://learn.microsoft.com/ja-jp/azure/site-recovery/site-recovery-test-failover-to-azure)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-4/vm-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-5 - VM ディスクに Managed Disks を使用します

**Category: Availability**

**Impact: High**

**Guidance**

Azure アンマネージド ディスクは、2025 年 9 月 30 日に完全に廃止されます。アンマネージド ディスクを使用する場合は、今すぐ移行の計画を開始してください。

**Resources**

- [Migrate your Azure unmanaged disks by Sep 30, 2025](https://learn.microsoft.com/ja-jp/azure/virtual-machines/unmanaged-disks-deprecation)
- [Migrate Windows VM from unmanaged disks to managed disks](https://learn.microsoft.com/ja-jp/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)
- [Migrate Linux VM from unmanaged disks to managed disks](https://learn.microsoft.com/ja-jp/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-5/vm-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-6 - データベース データをデータ ディスクにホストします

**Category: System Efficiency**

**Impact: Low**

**Guidance**

データベース データを OS ディスクではなくデータ ディスクでホストします。
データ ディスクは、保持する必要があるデータを格納するために仮想マシンに接続されるマネージド ディスクです。データ ディスクは SCSI ドライブとして登録され、選択した文字でラベル付けされます。データ ディスクでデータをホストすると、データのバックアップや復元の柔軟性が高まり、仮想マシンとオペレーティング システム全体を移行することなくディスクを移行できます。要件を満たす、さまざまな種類、サイズ、パフォーマンスの別のディスク SKU を選択できます。

**Resources**

- [Introduction to Azure managed disks - Data disks](https://learn.microsoft.com/ja-jp/azure/virtual-machines/managed-disks-overview#data-disk)
- [Azure managed disk types](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-types)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-6/vm-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-7 - Azure Backup サービスを使用して VM をバックアップします

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

仮想マシンのバックアップを有効にして、データをセキュリティで保護し、迅速に復元します。Azure Backup サービスは、データをバックアップし、Microsoft Azure クラウドから復旧するための、シンプルで安全、かつコスト効率の高いソリューションを提供します。

**Resources**

- [What is the Azure Backup service?](https://learn.microsoft.com/ja-jp/azure/backup/backup-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-7/vm-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-8 - 運用 VM では SSD ディスクを使用している必要があります

**Category: System Efficiency**

**Impact: High**

**Guidance**

Premium SSD ディスクは、I/O 集中型のアプリケーションや運用ワークロードに対して、高パフォーマンスで待機時間の短いディスク サポートを提供します。Standard SSD ディスクは、より低い IOPS レベルで一貫したパフォーマンスを必要とするワークロード向けに最適化された、コスト効率の高いストレージ オプションです。

次のことをお勧めします。

- Standard HDD ディスクは、開発/テストのシナリオと重要度の低いワークロードに最小のコストで使用します。
- Premium 対応 VM では、Standard HDD ディスクではなく Premium SSD ディスクを使用します。すべてのオペレーティング システム ディスクとデータ ディスクに Premium Storage を使用する単一インスタンス VM の場合、Azure は 99.9% 以上の VM 接続を保証します。

Standard HDD から Premium SSD ディスクにアップグレードする場合は、次の問題を考慮してください。

- アップグレードには VM の再起動が必要であり、このプロセスは完了するまでに 3 分から 5 分かかります。
- VM がミッション クリティカルな運用 VM の場合は、Premium ディスクのコストに対して可用性の向上を評価します。

This does not apply to ephemeral disks
**Resources**

- [Azure managed disk types](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-types#premium-ssd)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-8/vm-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-9 - 停止状態の VM を確認します

**Category: Governance**

**Impact: Low**

**Guidance**

Azure Virtual Machines (VM) インスタンスは、さまざまな状態を経ます。プロビジョニング状態と電源状態があります。仮想マシンが実行されていない場合は、仮想マシンに問題が発生している可能性があるか、不要になり、削除してコストを削減する可能性があることを示します。

**Resources**

- [States and billing status of Azure Virtual Machines](https://learn.microsoft.com/ja-jp/azure/virtual-machines/states-billing?context=%2Ftroubleshoot%2Fazure%2Fvirtual-machines%2Fcontext%2Fcontext#power-states-and-billing)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-9/vm-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-10 - 高速ネットワーク(AccelNet)を有効にします

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

高速ネットワークにより、VM へのシングル ルート I/O 仮想化 (SR-IOV) が可能になり、ネットワーク パフォーマンスが大幅に向上します。この高パフォーマンス パスは、データ パスからホストをバイパスし、サポートされている VM の種類で最も要求の厳しいネットワーク ワークロードの待機時間、ジッター、CPU 使用率を削減します。

この構成は必ずしも必要ではないため、ワークロードの要件に応じてこのオプションを評価してください。

**Resources**

- [Accelerated Networking (AccelNet) overview](https://learn.microsoft.com/ja-jp/azure/virtual-network/accelerated-networking-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-10/vm-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-11 - AccelNet が有効になっている場合は、GuestOS NIC ドライバを手動で更新する必要があります

**Category: Governance**

**Impact: Low**

**Guidance**

高速ネットワークを有効にすると、GuestOS の既定の Azure Virtual Network インターフェイスが Mellanox に置き換えられ、そのドライバーはサードパーティ ベンダーから提供されます。Microsoft が管理する Marketplace イメージは、最新バージョンの Mellanox ドライバーで提供されますが、仮想マシンがデプロイされた後は、ドライバーを最新の状態に維持する責任があります。

**Resources**

- [Accelerated Networking (AccelNet) overview](https://learn.microsoft.com/ja-jp/azure/virtual-network/accelerated-networking-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-11/vm-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-12 - VM にはパブリック IP を直接関連付けないでください

**Category: Access & Security**

**Impact: Medium**

**Guidance**

仮想マシンで送信インターネット接続が必要な場合は、NAT Gateway または Azure Firewall を使用することをお勧めします。どちらのサービスも可用性と SNAT ポートがはるかに高いため、サービスのセキュリティと回復性を高めるのに役立ちます。受信インターネット接続には、Azure Load Balancer や Application Gateway などの負荷分散ソリューションを使用することをお勧めします。

**Resources**

- [Use Source Network Address Translation (SNAT) for outbound connections](https://learn.microsoft.com/ja-jp/azure/load-balancer/load-balancer-outbound-connections)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-12/vm-12.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-13 - VM ネットワーク インターフェイスと関連付けられているサブネットの両方に、ネットワーク セキュリティ グループ (NSG) が関連付けられています

**Category: Access & Security**

**Impact: Low**

**Guidance**

特別な理由がない限り、ネットワーク セキュリティ グループをサブネットまたはネットワーク インターフェイスに関連付けることをお勧めしますが、両方には関連付けないでください。サブネットに関連付けられたネットワーク・セキュリティ・グループのルールは、ネットワーク・インタフェースに関連付けられたネットワーク・セキュリティ・グループのルールと競合する可能性があるため、予期しない通信の問題が発生し、トラブルシューティングが必要になる可能性があります。

**Resources**

- [How network security groups filter network traffic](https://learn.microsoft.com/ja-jp/azure/virtual-network/network-security-group-how-it-works#intra-subnet-traffic)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-13/vm-13.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-14 - IP 転送は、ネットワーク仮想アプライアンスに対してのみ有効にする必要があります

**Category: Access & Security**

**Impact: Medium**

**Guidance**

IP 転送により、仮想マシンのネットワーク インターフェイスで次のことが可能になります。

ネットワーク インターフェイスに割り当てられている IP 構成のいずれかに割り当てられている IP アドレスの 1 つを宛先としないネットワーク トラフィックを受信します。

ネットワーク インターフェイスの IP 構成の 1 つに割り当てられた送信元 IP アドレスとは異なる送信元 IP アドレスを使用してネットワーク トラフィックを送信します。

この設定は、仮想マシンが転送する必要があるトラフィックを受信する仮想マシンに接続されているすべてのネットワーク インターフェイスに対して有効にする必要があります。仮想マシンは、複数のネットワーク インターフェイスが接続されているか、単一のネットワーク インターフェイスが接続されているかに関係なく、トラフィックを転送できます。IP 転送は Azure の設定ですが、仮想マシンでは、ファイアウォール、WAN 最適化、負荷分散アプリケーションなど、トラフィックを転送できるアプリケーションも実行する必要があります。

**Resources**

- [Enable or disable IP forwarding](https://learn.microsoft.com/ja-jp/azure/virtual-network/virtual-network-network-interface?tabs=network-interface-portal#enable-or-disable-ip-forwarding)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-14/vm-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-15 - お客様の DNS サーバーは、仮想ネットワーク レベルで構成する必要があります

**Category: Storage**

**Impact: Low**

**Guidance**

仮想ネットワークで DNS サーバーを構成して、環境全体の不整合を回避します。

**Resources**

- [Name resolution for resources in Azure virtual networks](https://learn.microsoft.com/ja-jp/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-15/vm-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-16 - 共有ディスクは、クラスタ化されたサーバーでのみ有効にする必要があります

**Category: Storage**

**Impact: Medium**

**Guidance**

Azure 共有ディスクは、マネージド ディスクを複数の仮想マシン (VM) に同時に接続できる Azure マネージド ディスクの機能です。マネージド ディスクを複数の VM に接続すると、新しいクラスター化されたアプリケーションを Azure にデプロイしたり、既存のクラスター化されたアプリケーションを Azure に移行したりできますが、ディスクがクラスターの複数の仮想マシン メンバーに割り当てられる状況でのみ使用する必要があります。

**Resources**

- [Azure Shared Disk Introduction](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-shared)
- [Enable Shared Disks](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-shared-enable?tabs=azure-portal)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-16/vm-16.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-17 - VM ディスクへのネットワーク アクセスは、 パブリック アクセスを無効にし、プライベート アクセスを有効に設定する必要があります

**Category: Access & Security**

**Impact: Low**

**Guidance**

「パブリック アクセスを無効にしてプライベート アクセスを有効にする」に変更し、プライベート・エンドポイントを作成することをお勧めします。

**Resources**

- [Restrict import/export access for managed disks using Azure Private Link](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-enable-private-links-for-import-export-portal)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-17/vm-17.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-18 - VM が Azure ポリシーに準拠していることを確認します

**Category: Governance**

**Impact: Low**

**Guidance**

実行するアプリケーションに対して仮想マシン (VM) をセキュリティで保護することが重要です。VM のセキュリティ保護には、VM への安全なアクセスとデータの安全なストレージをカバーする 1 つ以上の Azure サービスと機能を含めることができます。この記事では、VM とアプリケーションをセキュリティで保護するための情報を提供します。

**Resources**

- [Policy-driven governance](https://learn.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/landing-zone/design-principles#policy-driven-governance)
- [Azure Policy Regulatory Compliance controls for Azure Virtual Machines](https://learn.microsoft.com/ja-jp/azure/virtual-machines/security-policy)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-18/vm-18.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-19 - マネージド ディスクの高度な暗号化オプションを有効にします

**Category: Access & Security**

**Impact: Medium**

**Guidance**

Azure Disk Storage サーバー側の暗号化 (保存時の暗号化または Azure Storage 暗号化とも呼ばれます) は、ストレージ クラスターに保持されるときに、Azure マネージド ディスク (OS およびデータ ディスク) に格納されているデータを自動的に暗号化します。マネージド ディスクで使用できる高度な暗号化オプションには、Azure Disk Encryption (ADE)、ホストでの暗号化、機密ディスクの暗号化など、いくつかの種類があります。

- ADE は、Linux の DM-Crypt 機能または Windows の BitLocker 機能を使用して、VM 内の Azure 仮想マシン (VM) のディスクを暗号化します。
- ホストでの暗号化により、VM をホストしている VM ホストに格納されているデータが保存時に暗号化され、暗号化されたストレージ クラスターに流れるようになります。
- 機密ディスクの暗号化は、ディスク暗号化キーを仮想マシンの TPM にバインドし、保護されたディスク コンテンツに VM からのみアクセスできるようにします。


**Resources**

- [Overview of managed disk encryption options](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disk-encryption-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-19/vm-19.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-20 - VM Insights を有効にします

**Category: Monitoring**

**Impact: Low**

**Guidance**

VM insights は、仮想マシンと仮想マシン スケール セットのパフォーマンスと正常性を監視します。実行中のプロセスと他のリソースへの依存関係を監視します。VM insights は、パフォーマンスのボトルネックとネットワークの問題を特定することで、重要なアプリケーションの予測可能なパフォーマンスと可用性を提供するのに役立ちます。また、問題が他の依存関係に関連しているかどうかを理解するのにも役立ちます。

**Resources**

- [Overview of VM insights](https://learn.microsoft.com/ja-jp/azure/azure-monitor/vm/vminsights-overview)
- [Did the extension install properly?](https://learn.microsoft.com/ja-jp/azure/azure-monitor/vm/vminsights-troubleshoot#did-the-extension-install-properly)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-20/vm-20.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-21 - すべての Azure Virtual Machines の診断設定を構成します

**Category: Monitoring**

**Impact: Low**

**Guidance**

プラットフォーム メトリックは、既定で構成なしで Azure Monitor メトリックに自動的に送信されます。
プラットフォーム ログは、Azure リソースとそれらが依存する Azure プラットフォームに関する詳細な診断および監査情報を提供します。

- リソース ログは、宛先にルーティングされるまで収集されません。
- アクティビティ ログは単独で存在しますが、他の場所にルーティングできます。

各 Azure リソースには、次の条件を定義する独自の診断設定が必要です。

- ソース:設定で定義された宛先に送信するメトリックおよびログデータのタイプ。使用可能なタイプは、リソースタイプによって異なります。
- 宛先: 送信先の 1 つ以上の宛先。

1 つの診断設定では、各宛先を 1 つしか定義できません。特定の宛先の種類の複数 (たとえば、2 つの異なる Log Analytics ワークスペース) にデータを送信する場合は、複数の設定を作成します。各リソースには、最大 5 つの診断設定を含めることができます。

**Resources**

- [Diagnostic settings in Azure Monitor](https://learn.microsoft.com/ja-jp/azure/azure-monitor/essentials/diagnostic-settings?tabs=portal)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-21/vm-21.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-22 - VM のメンテナンス構成を使用します

**Category: Governance**

**Impact: High**

**Guidance**

メンテナンス構成設定を使用すると、ユーザーは更新をスケジュールおよび管理し、VM の更新/中断が計画された時間枠内に確実に行われるようにすることができます。

**Resources**

- [Use maintenance configurations to control and manage the VM updates](https://learn.microsoft.com/ja-jp/azure/virtual-machines/maintenance-configurations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-22/vm-22.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-23 - CPU の完全なパフォーマンスを継続的に必要とする運用 VM には、A または B シリーズの VM SKU を使用しないでください

**Category: System Efficiency**

**Impact: High**

**Guidance**

A シリーズの VM には、開発やテストなどのエントリ レベルのワークロードに最適な CPU パフォーマンスとメモリ構成があります。ユースケースの例としては、開発サーバーとテストサーバー、低トラフィックのWebサーバー、小規模から中規模のデータベース、概念実証、コードリポジトリなどがあります。

B シリーズの VM は、Web サーバー、概念実証、小規模なデータベース、開発ビルド環境など、CPU の完全なパフォーマンスを継続的に必要としないワークロードに最適です。これらのワークロードには、通常、バースト可能なパフォーマンス要件があります。このサイズがデプロイされている物理ハードウェアを特定するには、仮想マシン内から仮想ハードウェアを照会します。B シリーズでは、ベースライン パフォーマンスの VM サイズを購入して、ベースラインよりも少ない使用量のときにクレジットを蓄積できます。VM にクレジットが蓄積されると、アプリケーションでより高い CPU パフォーマンスが必要な場合、VM は vCPU の最大 100% を使用してベースラインを超えてバーストできます。すべての CPU クレジットを消費すると、B シリーズの仮想マシンは、CPU バーストにクレジットが蓄積されるまで、基本 CPU パフォーマンスに調整されます。

**Resources**

- [B-series burstable virtual machine sizes](https://learn.microsoft.com/ja-jp/azure/virtual-machines/sizes-b-series-burstable)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-23/vm-23.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-24 - ミッション クリティカルなワークロードでは、Premium ディスクまたは Ultra ディスクを使用する必要があります

**Category: System Efficiency**

**Impact: High**

**Guidance**

Azure Premium SSD は、入出力 (IO) 集中型のワークロードを持つ仮想マシン (VM) に対して、高パフォーマンスで低遅延のディスク サポートを提供します。

Premium SSD v2 は、Premium SSD よりも高いパフォーマンスを提供しながら、一般的にコストも低くなります。Premium SSD v2 ディスクのパフォーマンス (容量、スループット、IOPS) はいつでも個別に調整できるため、変化するパフォーマンス ニーズに対応しながら、ワークロードのコスト効率を高めることができます。V2 は OS ディスクとしてサポートされていないため、Premium ソリッド ステート ドライブ (SSD) をオペレーティング システム (OS) ディスクとして使用する必要があります。

Azure Ultra Disks は、Azure 仮想マシン (VM) 向けの最高パフォーマンスのストレージ オプションです。Ultra ディスクのパフォーマンス パラメーターは、VM を再起動せずに変更できます。 Ultra ディスクは、SAP HANA、最上位データベース、トランザクション負荷の高いワークロードなど、データ集約型のワークロードに適しています。Ultra ディスクはデータ ディスクとして使用する必要があり、空のディスクとしてのみ作成できます。Premium ソリッド ステート ドライブ (SSD) は、オペレーティング システム (OS) ディスクとして使用する必要があります。

**Resources**

- [Disk type comparison and decision tree](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-types#disk-type-comparison)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-24/vm-24.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-27 - メンテナンスの機密性の高いワークロードに Azure Boost VM を使用します

**Category: Availability**

**Impact: Medium**

**Guidance**

ワークロードがメンテナンスの影響を受けやすい場合は、Azure Boost 互換 VM の使用を検討してください。 Azure Boost は、Azure メンテナンス アクティビティが発生したときのお客様への影響を軽減するように設計されています。

**Resources**

- [Microsoft Azure Boost](https://learn.microsoft.com/ja-jp/azure/azure-boost/overview)
- [Announcing the general availability of Azure Boost](https://aka.ms/AzureBoostGABlog)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-27/vm-27.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### VM-28 - メンテナンスの影響を受けやすいワークロードの仮想マシンは、スケジュールされたイベントを有効化します

**Category: Availability**

**Impact: Medium**

**Guidance**

ワークロードがメンテナンスの影響を受けやすい場合は、スケジュールされたイベントを有効にしてください。スケジュールされたイベントは、仮想マシンのメンテナンスを準備するための時間をアプリケーションに与える Azure メタデータ サービスです。今後のメンテナンス イベント (再起動など) に関する情報が提供されるため、アプリケーションはそれらに備え、中断を制限できます。これは、Windows と Linux の両方の PaaS と IaaS を含む、すべての種類の Azure Virtual Machines で使用できます。

**Resources**

- [Monitor scheduled events for your Azure VMs](https://learn.microsoft.com/ja-jp/azure/virtual-machines/windows/scheduled-event-service)
- [Azure Metadata Service: Scheduled Events for Linux VMs](https://learn.microsoft.com/ja-jp/azure/virtual-machines/linux/scheduled-events)
- [Azure Metadata Service: Scheduled Events for Windows VMs](https://learn.microsoft.com/ja-jp/azure/virtual-machines/windows/scheduled-events)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/vm-28/vm-28.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
