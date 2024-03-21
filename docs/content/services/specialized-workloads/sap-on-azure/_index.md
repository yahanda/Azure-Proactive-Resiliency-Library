+++
title = "SAP on Azure"
description = "Best practices and resiliency recommendations for Azure Sap Solution and associated resources and settings."
date = "2/13/24"
author = "humblejay"
msAuthor = "kupole"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure Sap Solution and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |                                Category                                 |     Impact      |      State       | ARG Query Available |
|:--------------------------------------------------|:-----------------------------------------------------------------------:|:---------------:|:----------------:|:-------------------:|
| [SAP-1 - Ensure that each SAP production system is designed for high availability using availability zones.](#sap-1---ensure-that-each-sap-production-systems-are-designed-for-high-availability) | Availability | High | Verified | No |
| [SAP-2 - Run SAP application servers on two or more VMs using VMSS Flex.](#sap-2---run-sap-application-servers-on-two-or-more-vms-using-vmss-flex) | Availability | High | Verified | No  |
| [SAP-6 - Avoid placing application server and database VMs in one Proximity Placement Group.](#sap-6---avoid-placing-application-server-and-database-in-one-proximity-placement-group) | Availability | High | Verified |  No  |
| [SAP-7 - Avoid placing VMs from multiple SAP systems in a single Proximity Placement Group.](#sap-7---avoid-placing-vms-from-multiple-sap-systems-in-a-single-proximity-placement-group) | Availability | High | Verified | No |
| [SAP-8 - When creating availability sets, ensure that you have maximum number of fault domains and a sufficient number of update domains.](#sap-8---when-creating-availability-sets-ensure-that-you-have-maximum-number-of-fault-domains-and-a-sufficient-number-of-update-domains) | Availability | High | Verified | No |
| [SAP-9 - If using single-instance VMs, all OS and data disks must be Premium SSD or Ultra Disk.](#sap-9---if-using-single-instance-vms-all-os-and-data-disks-must-be-premium-ssd-or-ultra-disk) | Availability | High | Verified |  No  |
| [SAP-14 - Ensure that each database replicates changes synchronously (SYNC mode) to a stand-by node.](#sap-14---ensure-that-the-data-is-replicated-synchronously-sync-mode-between-the-primary-and-secondary-database-hosting-vm-nodes) | Availability | High | Verified |  No  |
| [SAP-15 - Ensure that SAP shared file systems are designed for high availability, and when possible using availability zones.](#sap-15---ensure-that-the-sap-shared-files-systems-are-made-highly-available) | Availability | High | Verified |  No  |
| [SAP-16 - Test high availability solutions thoroughly to ensure fail overs work as expected.](#sap-16---test-high-availability-solutions-thoroughly-to-ensure-fail-overs-work-as-expected) | Availability | High | Verified | No |
| [SAP-18 - Remove unwanted location constraints from your Linux Pacemaker clusters.](#sap-18---remove-unwanted-location-constraints-from-your-linux-pacemaker-clusters) | Availability | High | Verified | No |
| [SAP-22 - Protect SAP production workloads with a cross-region disaster recovery solution.](#sap-22---protect-sap-production-workloads-with-a-cross-region-disaster-recovery-solution) | Disaster Recovery | High | Verified |  No  |
| [SAP-24 - Implement an offsite backup strategy for production workloads by utilizing the second Azure region for backups.](#sap-24---implement-an-offsite-backup-strategy-for-production-workloads-by-utilizing-the-second-azure-region-for-backups) | Disaster Recovery | High | Verified | No  |
| [SAP-26 - Secure compute resource capacity for critical VM roles in DR region.](#sap-26---secure-compute-resource-capacity-for-critical-vm-roles-in-dr-region) | Disaster Recovery | Medium | Verified | No |
| [SAP-27 - Ensure that the production databases are replicated (ASYNC) to DR location, use database vendor's replication.](#sap-27---ensure-that-the-production-databases-are-replicated-async-to-dr-location-use-database-vendors-replication) | Disaster Recovery | High | Verified | No |
| [SAP-28 - SAP components are backed up to DR location using an appropriate backup tool or ASR.](#sap-28---sap-components-are-backed-up-to-dr-location-using-an-appropriate-backup-tool-or-asr) | Disaster Recovery | High | Verified | No |
| [SAP-29 - SAP shared files systems are replicated or backed up to DR location.](#sap-29---sap-shared-files-systems-and-any-other-critical-to-dr-are-replicated-or-backed-up-to-dr-location) | Disaster Recovery | High | Verified | No |
| [SAP-32 - Automate DR infrastructure build or pre-deploy DR resources.](#sap-32---automate-dr-infrastructure-build-or-pre-deploy-dr-resources) | Disaster Recovery | Medium | Verified |  No  |
| [SAP-33 - Document and test DR procedure, ensure it meets RPO and RTO targets.](#sap-33---document-and-test-dr-procedure-ensure-it-meets-rpo-and-rto-targets) | Disaster Recovery | Medium | Verified | No |
| [SAP-34 - Ensure there is a robust monitoring and alerting solution in place for the entire DR solution.](#sap-34---ensure-there-is-a-robust-monitoring-and-alerting-solution-in-place-for-the-entire-dr-solution) | Disaster Recovery | Medium | Verified | No |
| [SAP-36 - Configure scheduled events so you are notified of upcoming maintenance events.](#sap-36---configure-scheduled-events-notification) | Monitoring | High | Verified | No |
| [SAP-42 - ASCS-Pacemaker (Central Server Instance) Ensure Pacemaker cluster has been setup for SAP ASCS high availability.](#sap-42---ascs-pacemaker-central-server-instance-ensure-pacemaker-cluster-has-been-setup-for-sap-ascs-high-availability) | Automation | High | Verified | No |
| [SAP-43 - ASCS-Pacemaker-SLES (Central Server Instance) Ensure the Pacemaker cluster has been setup for SAP ASCS high availability.](#sap-43---ascs-pacemaker-sles-central-server-instance-ensure-the-pacemaker-cluster-has-been-setup-for-sap-ascs-high-availability) | Availability | High | Verified | No  |
| [SAP-44 - ASCS-Pacemaker-RH (Central Server Instance) Ensure the Pacemaker cluster has been setup for SAP ASCS high availability.](#sap-44---ascs-pacemaker-rh-central-server-instance-ensure-the-pacemaker-cluster-has-been-setup-for-sap-ascs-high-availability) | Availability | High | Verified | No  |
| [SAP-45 - ASCS-LB (Central Server Instance) Ensure the load balancer is configured correctly for SAP ASCS High availability.](#sap-45---ascs-lb-central-server-instance-ensure-the-load-balancer-is-configured-correctly-for-sap-ascs-high-availability) | Availability | High | Verified | No |
| [SAP-46 - DBHANA-Pacemaker (Database Instance) Ensure the Pacemaker cluster has been setup for SAP HANA DB high availability.](#sap-46---dbhana-pacemaker-database-instance-ensure-the-pacemaker-cluster-has-been-setup-for-sap-hana-db-high-availability) | Availability | High | Verified | No |
| [SAP-49 - DBHANA-LB (Database Instance) Ensure the load balancer is configured correctly for SAP HANA DB High availability.](#sap-49---dbhana-lb-database-instance-ensure-the-load-balancer-is-configured-correctly-for-sap-hana-db-high-availability) | Availability | High | Verified | No |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### SAP-1 - 各 SAP 運用システムが高可用性を実現するように設計されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

Azure Availability Zones は、各 Azure リージョン内の物理的に分離された場所であり、ローカルの障害に耐えられます。アベイラビリティーゾーンを使用して、データセンターの予期せぬ障害からアプリケーションとデータを保護します。複数の可用性ゾーンを使用して、各 SAP 運用システムの各単一障害点が高可用性で保護されるようにします。リージョン内の異なるゾーンにデプロイできない場合は、SAP ワークロードの高可用性デプロイ オプションに関する Microsoft のガイダンスを参照してください。

**Resources**

- [Move Regional SAP HA to Zonal](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/Move-VM-from-AvSet-to-AvZone/Move-Regional-SAP-HA-To-Zonal-SAP-HA-WhitePaper)
- [High Availability Deployment Options for SAP](https://learn.microsoft.com/ja-jp/azure/sap/workloads/sap-high-availability-architecture-scenarios#high-availability-deployment-options-for-sap-workload)
- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-1/sap-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-2 - VMSS Flex を使用して 2 つ以上の VM で SAP アプリケーション サーバーを実行します

**Category: Availability**

**Impact: High**

**Guidance**

柔軟なオーケストレーションで Virtual Machines スケール セット (VMSS) を使用して、指定したゾーン間および各ゾーン内で仮想マシンを分散し、ゾーン内の異なる障害ドメインにベスト エフォート ベースで VM を分散します。適切なモードと正しい設定を使用して、SAP ワークロードに関する Microsoft の推奨事項に従って VMSS Flex を構成します。現在、VMSS Flex for SAP アプリケーション サーバーを使用しておらず、障害ドメインと更新ドメインの分散で可用性セットも使用していない場合は、VMSS Flex アーキテクチャへの移行を検討して、SAP デプロイの回復性体制を改善する必要があります。以下のリンクにある次のブログ記事では、可用性セットまたは可用性ゾーンにデプロイされている既存の SAP ワークロードを、FD=1 デプロイ オプションを使用してフレキシブル スケール セットに移行するプロセスの詳細について概説しています。


**Resources**

- [Virtual machine Scale Set SAP Deployment Guide](https://learn.microsoft.com/ja-jp/azure/sap/workloads/virtual-machine-scale-set-sap-deployment-guide)
- [Considerations for Flexible VM Scale Sets for SAP](https://learn.microsoft.com/ja-jp/azure/sap/workloads/virtual-machine-scale-set-sap-deployment-guide?tabs=scaleset-cli#important-consideration-of-flexible-virtual-machine-scale-sets-for-sap-workload)
- [Migrate existing SAP system VMs to VMSS Flex](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/how-to-easily-migrate-an-existing-sap-system-vms-to-flexible/ba-p/3833548)
- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-2/sap-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-6 - アプリケーション サーバーとデータベースを 1 つの近接通信配置グループに配置することは避けてください

**Category: Availability**

**Impact: High**

**Guidance**

Proximity Placement Group (PPG) is a deployment constraint, therefore placing special VM families such as M- or Mv2- series or placing a large number of VMs in one PPG may lead to allocation failures.

**Resources**

- [Proximity Placement Scenarios](https://learn.microsoft.com/ja-jp/azure/sap/workloads/proximity-placement-scenarios)
- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-6/sap-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-7 - 複数の SAP システムの VM を 1 つの近接配置グループに配置しないようにします

**Category: Availability**

**Impact: High**

**Guidance**

Ensure that VMs from different SAP systems are not colocated within a single Proximity Placement Group

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-7/sap-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-8 - 可用性セットを作成するときは、障害ドメインの最大数と十分な数の更新ドメインがあることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

The default number of fault domains is 2 and changing it later is not possible online.
Important! If you are currently using Availability Sets or Regional VMs for SAP application servers, then you should consider moving to Availability Zones and VMSS Flex architecture to improve the resiliency posture of your SAP deployment. For the details on the process of migrating existing SAP workloads that are deployed in an availability set or availability zone to a flexible scale set with FD=1 deployment option, refer to our public documentation.

**Resources**

- [How availability sets work](https://learn.microsoft.com/ja-jp/azure/virtual-machines/availability-set-overview#how-do-availability-sets-work)
- [Migrate existing SAP system to VMSS Flex](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/how-to-easily-migrate-an-existing-sap-system-vms-to-flexible/ba-p/3833548)
- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-8/sap-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-9 - 単一インスタンス VM を使用する場合、すべての OS ディスクとデータ ディスクが Premium SSD または Ultra Disk である必要があります

**Category: Availability**

**Impact: High**

**Guidance**

For single-instance VMs, both OS and data disks must be either Premium SSD or Ultra Disk to achieve the single-instance SLA of 99.9% availability.

**Resources**

- [VM SLA](https://www.azure.cn/en-us/support/sla/virtual-machines/)
- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-9/sap-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-14 - VM ノードをホストしているプライマリ データベースとセカンダリ データベース間でデータが同期的にレプリケートされる (SYNC モード) ことを確認します

**Category: Availability**

**Impact: High**

**Guidance**

High availability for databases should be implemented using database native replication technologies and the data should be replicated synchronously that is in SYNC mode from primary database to a stand-by node.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-14/sap-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-15 - SAP 共有ファイルシステムの高可用性を確保します

**Category: Availability**

**Impact: High**

**Guidance**

SAP shared file systems such as /sapmnt, /usr/trans, interfaces should be made highly available.
In case of Azure File Shares, we recommend that you use ZRS (Zone-redundant storage) and for Azure NetApp Files use Zonal replication for your volumes.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-15/sap-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-16 - 高可用性ソリューションを徹底的にテストして、フェールオーバーが期待どおりに機能することを確認します

**Category: Availability**

**Impact: High**

**Guidance**

Test all high availability solutions thoroughly (including kernel panic in Linux VMs and also fail-back). Include zonal failure scenarios in your testing, the testing should confirm that each layer of your SAP solution including database, central services, application servers and shared file systems is configured correctly for zone redundancy, the solution meets RPO = 0 and the application fails over automatically meeting your RTO.
The fail back can be either automatic or manual.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-16/sap-16.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-18 - Linux Pacemaker クラスターから不要な場所の制約を削除します

**Category: Availability**

**Impact: High**

**Guidance**

When executing a migrate command in a Linux Pacemaker cluster, the system generates a temporary "prefer" location constraint, aiming to move a resource to a specified node. This constraint prioritizes the target node for the resource temporarily without permanently altering the cluster’s configuration.

During planned maintenances and fail over testing, you can leverage the migrate command for temporary resource relocation during maintenance or administrative tasks to ensure minimal disruption. This constraint is not permanent and does not survive reboots or cluster resets. It's designed for short-term adjustments.

Once the planned task necessitating the resource migration is complete, manually remove the temporary constraint to revert to the cluster's original resource management policies.
This approach allows for controlled resource movement within the cluster, facilitating maintenance while preserving the integrity and efficiency of the cluster's configuration.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-18/sap-18.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-22 - SAP の本番ワークロードをクロスリージョンのディザスタリカバリソリューションで保護します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

To safeguard SAP production workloads against catastrophic events it is imperative to implement a cross-region disaster recovery solution. This approach ensures business continuity by replicating data and applications across geographically diverse regions, minimizing the risk of data loss and downtime in the event of a regional outage.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-22/sap-22.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-24 - バックアップに 2 番目の Azure リージョンを利用して、運用ワークロードのオフサイト バックアップ戦略を実装します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

To bolster your data protection strategy and ensure business continuity, it is important to implement an offsite backup strategy for production workloads leveraging a second Azure region. This approach enhances data security and business continuity by providing geographical redundancy and improved disaster recovery capabilities. This approach also often supports compliance with regulatory requirements ensuring your organization's resilience against data loss and operational disruptions.

Ensure that any Azure Recovery Services vaults used for backing up production VMs are GRS.

In addition, ensure that each of the following technical layers of an SAP system are protected with a suitable backup strategy and backups are also stored in the second region.

Virtual Machine backups
Database
SAP Central Services
SAP Shared File Systems

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-24/sap-24.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-26 - DR リージョン内の重要な VM ロールのコンピューティング リソース容量を確保します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

To ensure the availability of compute resources for critical VM roles in a DR region, consider securing capacity either through a warm standby approach or by utilizing Azure's On-demand Capacity Reservation.

Warm standby involves keeping VMs in the DR region running. On-demand Capacity Reservation, on the other hand, reserves compute capacity without having to run the VMs, allowing you to start them when needed. When DR VMs are not needed, the reserved capacity may safely be used to run other workloads without the risk of losing the capacity to other customers. This strategy guarantees resource availability for your critical workloads in the event of a disaster, balancing cost and readiness.

**Resources**

- [Capacity reservation overview](https://learn.microsoft.com/ja-jp/azure/virtual-machines/capacity-reservation-overview)
- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-26/sap-26.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-27 - 運用データベースが DR の場所にレプリケート (ASYNC) されていることを確認し、データベース ベンダーのレプリケーションを使用します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Replicate production databases (ASYNC) to the DR location using the database vendor’s replication technology.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-27/sap-27.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-28 - SAP コンポーネントは、適切なバックアップ ツールまたは ASR を使用して DR の場所にバックアップされます

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

SAP components such as (A)SCS, application servers, WebDispatchers, etc are backed up to DR location using an appropriate backup tool or ASR.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-28/sap-28.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-29 - SAP 共有ファイル システムと DR にとって重要なその他のものが DR の場所にレプリケートまたはバックアップされます

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Ensure that critical SAP shared file systems, such as /sapmnt, /usr/trans and /interfaces are either replicated or backed up for disaster recovery purposes.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-29/sap-29.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-32 - DR インフラストラクチャの構築または DR リソースの事前デプロイを自動化します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

Automate the build of disaster recovery (DR) infrastructure (or pre-deploy DR resources) and streamline SAP service recovery as much as possible.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-32/sap-32.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-33 - DR 手順を文書化してテストし、RPO と RTO の目標を満たしていることを確認します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

Create detailed documentation of your DR procedures for each layer of the SAP architecture—database, central services, application servers, and shared file systems. This documentation should include configuration details, failover mechanisms, and step-by-step recovery procedures.

Test a wide range of failure scenarios, including regional outages. Testing should confirm that your DR strategy is robust, meets your RPO and RTO targets, and provides seamless failover across all layers of the SAP architecture. This will ensure a comprehensive and resilient DR strategy capable of withstanding regional failures and ensuring business continuity.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-33/sap-33.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-34 - DR ソリューション全体に対して堅牢な監視およびアラートソリューションが導入されていることを確認します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

For an SAP solution hosted on Azure it is imperative to implement a robust monitoring and alerting solution that comprehensively covers DR of each layer of the SAP architecture. Given the complexity of SAP systems, which span multiple layers using diverse technologies and Azure resources, each with potentially distinct DR replication mechanisms, an appropriate monitoring strategy is crucial. The different layers include database, central services, application, and shared file systems.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/en-us/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-34/sap-34.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-36 - スケジュールされたイベントの通知を構成します

**Category: Monitoring**

**Impact: High**

**Guidance**

Scheduled events is an Azure Metadata Services that provides proactive notifications about upcoming maintenance events (for example, reboot) so that your application can prepare for them and limit disruption. You should configure scheduled events for all your critical Azure VMs.


Resource agent azure-events-az can also integrate with Pacemaker clusters.

To ensure high availability and service continuity in your Azure VMs, you should configure the azure-events-az resource agent within your Pacemaker clusters. This agent monitors for scheduled Azure maintenance events and can proactively relocate resources for a graceful node shutdown. Configure the agent to monitor specific event types such as Reboot and Redeploy, and enable verbose logging for detailed diagnostics.



In addition, it is also important that you define a procedure on how to react to scheduled events.

**Resources**

- [VM Scheduled Events](https://learn.microsoft.com/ja-jp/azure/virtual-machines/linux/scheduled-events)
- [Configure Pacemaker for Azure Scheduled Events](https://learn.microsoft.com/ja-jp/azure/sap/workloads/high-availability-guide-suse-pacemaker?tabs=msi#configure-pacemaker-for-azure-scheduled-events)
- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-36/sap-36.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-42 - ASCS-Pacemaker (セントラル サーバー インスタンス) Pacemaker クラスターが SAP ASCS 高可用性用にセットアップされていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

For the ASCS-Pacemaker (Central Server Instance), ensure that the Pacemaker cluster configuration parameters are correctly set up for SAP ASCS high availability.

**Resources**

- [SAP ACSS checks](https://learn.microsoft.com/ja-jp/azure/sap/center-sap-solutions/get-quality-checks-insights)
- [OpenSource Quality checks](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/main/QualityCheck)
- [ASCS-Pacemaker - Central Server Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-42/sap-42.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-43 - ASCS-Pacemaker-SLES (セントラル サーバー インスタンス) Pacemaker クラスターが SAP ASCS 高可用性用にセットアップされていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

For the ASCS-Pacemaker-SLES (Central Server Instance), ensure that the Pacemaker cluster configuration parameters are correctly set up for SAP ASCS high availability when running on SLES.

**Resources**

- [ASCS-Pacemaker-SLESCentral Server Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-43/sap-43.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-44 - ASCS-Pacemaker-RH (セントラル サーバー インスタンス) Pacemaker クラスターが SAP ASCS 高可用性用にセットアップされていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

For the ASCS-Pacemaker-RH (Central Server Instance), ensure that the Pacemaker cluster configuration parameters are correctly set up for SAP ASCS high availability when running on Red Hat.

**Resources**

- [ASCS-Pacemaker-RH Central Server Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-44/sap-44.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-45 - ASCS-LB (セントラル サーバー インスタンス) ロード バランサーが SAP ASCS 高可用性用に正しく構成されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

For the ASCS-LB (Central Server Instance), ensure that the load balancer is configured correctly for SAP ASCS high availability.

**Resources**

- [ASCS-LB - Central Server Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-45/sap-45.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-46 - DBHANA-Pacemaker (データベース インスタンス) Pacemaker クラスターが SAP HANA DB の高可用性用にセットアップされていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

For the DBHANA-Pacemaker (Database Instance), ensure that the Pacemaker cluster configuration parameters are correctly set up for SAP HANA DB high availability.

**Resources**

- [DBHANA-Pacemaker - Database Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-46/sap-46.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### SAP-49 - DBHANA-LB (データベース インスタンス) ロードバランサーが SAP HANA DB 高可用性用に正しく構成されていることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

For the DBHANA-LB (Database Instance), make sure the load balancer is configured correctly for SAP HANA DB high availability.

**Resources**

- [DBHANA-LB- Database Instance](https://docs.microsoft.com/ja-jp/azure/advisor/advisor-reference-reliability-recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/sap-49/sap-49.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
