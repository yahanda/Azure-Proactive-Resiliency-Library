+++
title = "Azure NetApp Files"
description = "Best practices and resiliency recommendations for Azure NetApp Files and associated resources and settings."
date = "3/26/24"
author = "seanluce"
msAuthor = "b-sluce"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure NetApp Files and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State   | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------: | :-----------------: |
| [ANF-1 - Use the correct service level and volume quota size for the expected performance level](#anf-1---use-the-correct-service-level-and-volume-quota-size-for-the-expected-performance-level) | System Efficiency | Medium | Verified  |         No         |
| [ANF-2 - Use standard network features for production in Azure NetApp Files](#anf-2---use-standard-network-features-for-production-in-azure-netapp-files) | Networking | High | Verified  |         Yes         |
| [ANF-3 - Use availability zones for high availability in Azure NetApp Files](#anf-3---use-availability-zones-for-high-availability-in-azure-netapp-files) | Availability | High | Verified  |         Yes         |
| [ANF-4 - Use snapshots for data protection in Azure NetApp Files](#anf-4---use-snapshots-for-data-protection-in-azure-netapp-files) | Availability | High | Verified  |         Yes         |
| [ANF-5 - Enable backup for data protection in Azure NetApp Files](#anf-5---enable-backup-for-data-protection-in-azure-netapp-files) | Disaster Recovery | High | Verified  |         Yes         |
| [ANF-6 - Enable Cross-region replication of Azure NetApp Files volumes](#anf-6---enable-cross-region-replication-of-azure-netapp-files-volumes) | Disaster Recovery | High | Verified  |         Yes         |
| [ANF-7 - Enable Cross-zone replication of Azure NetApp Files volumes](#anf-7---enable-cross-zone-replication-of-azure-netapp-files-volumes) | Availability | High | Verified  |         Yes         |
| [ANF-8 - Monitor Azure NetApp Files metrics to better understand usage pattern and performance](#anf-8---monitor-azure-netapp-files-metrics-to-better-understand-usage-pattern-and-performance) | Monitoring | Medium | Verified  |         No         |
| [ANF-9 - Use Azure policy to enforce organizational standards and to assess compliance at-scale in Azure NetApp Files](#anf-9---use-azure-policy-to-enforce-organizational-standards-and-to-assess-compliance-at-scale-in-azure-netapp-files) | Governance | Medium | Verified  |         No         |
| [ANF-10 - Restrict default access to Azure NetApp Files volumes](#anf-10---restrict-default-access-to-azure-netapp-files-volumes) | Access & Security | Medium | Verified  |         No         |
| [ANF-11 - Make use of SMB continuous availability for supported applications](#anf-11---make-use-of-smb-continuous-availability-for-supported-applications) | Application Resilience | Medium | Verified  |         No         |
| [ANF-12 - Ensure application resilience for service maintenance events](#anf-12---ensure-application-resilience-for-service-maintenance-events) | Application Resilience | Medium | Verified  |         No         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ANF-1 - 予想されるパフォーマンスレベルに対して正しいサービスレベルとボリュームクォータサイズを使用します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

サービス レベルは、容量プールの属性です。サービス レベルは、ボリュームに割り当てられているクォータに基づいて、容量プール内のボリュームに対して許容される最大スループットによって定義され、区別されます。スループットは、読み取り速度と書き込み速度の組み合わせです。Azure NetApp Files では、次の 3 つのサービス レベルがサポートされています。

- 標準 (1TiB あたり 16 MiB/秒) スループット
- Premium (1TiB あたり 64 MiB/秒) スループット
- Ultra (1TiB あたり 128 MiB/秒) スループット

**Resources**

- [Service levels for Azure NetApp Files | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/azure-netapp-files-service-levels)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-1/anf-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-2 - Azure NetApp Files での運用環境に Standard ネットワーク機能を使用します

**Category: Networking**

**Impact: High**

**Guidance**

標準ネットワーク機能を使用すると、より高い IP 制限と、委任されたサブネット上のネットワーク セキュリティ グループとユーザー定義ルート、追加の接続パターンなどの標準 VNet 機能が可能になります。

**Resources**

- [Guidelines for Azure NetApp Files network planning | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/azure-netapp-files-network-topologies)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-2/anf-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-3 - Azure NetApp Files で高可用性を実現するために可用性ゾーンを使用します

**Category: Availability**

**Impact: High**

**Guidance**

Azure 可用性ゾーンは、ローカルの障害に耐えられる、サポートする各 Azure リージョン内の物理的に分離された場所です。障害は、ソフトウェアやハードウェアの障害から、地震、洪水、火災などのイベントまで多岐にわたります。障害に対する耐性は、Azure サービスの冗長性と論理的な分離によって実現されます。回復性を確保するために、すべての可用性ゾーン対応リージョンに少なくとも 3 つの個別の可用性ゾーンが存在します。

**Resources**

- [Use availability zones for high availability in Azure NetApp Files | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/use-availability-zones)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-3/anf-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-4 - Azure NetApp Files でのデータ保護にスナップショットを使用します

**Category: Availability**

**Impact: High**

**Guidance**

Azure NetApp Files スナップショット テクノロジは、パフォーマンスに影響を与えることなく、安定性、スケーラビリティ、迅速な復旧性を提供します。スナップショット ポリシーを使用して、Azure NetApp Files データのスナップショットを自動的に作成します。

**Resources**

- [How Azure NetApp Files snapshots work | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/snapshots-introduction)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-4/anf-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-5 - Azure NetApp Files でデータ保護のためのバックアップを有効にします

**Category: Availability**

**Impact: High**

**Guidance**

Azure NetApp Files は、長期的な復旧、アーカイブ、コンプライアンスのためのフル マネージド バックアップ ソリューションをサポートしています。バックアップは、バックアップと同じリージョン内の新しいボリュームに復元できます。Azure NetApp Files によって作成されたバックアップは、短期的な復旧や複製に使用できるボリューム スナップショットとは無関係に、Azure Storage に格納されます。バックアップ ポリシーを使用して、Azure NetApp Files データのバックアップを自動的に作成します。

**Resources**

- [Understand Azure NetApp Files backup | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/backup-introduction)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-5/anf-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-6 - Azure NetApp Files ボリュームのリージョン間レプリケーションを有効にします

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Azure NetApp Files レプリケーション機能は、リージョン間ボリューム レプリケーションによるデータ保護を提供します。あるリージョンの Azure NetApp Files ボリューム (ソース) から、別のリージョンの別の Azure NetApp Files ボリューム (宛先) にデータを非同期的にレプリケートできます。この機能により、リージョン全体の停止や災害が発生した場合に、重要なアプリケーションをフェールオーバーできます。

注: ボリュームは、クロスゾーンレプリケーション (CZR) またはクロスリージョンレプリケーション (CRR) を介してレプリケートできますが、両方を同時にレプリケートすることはできません。

**Resources**

- [Cross-region replication of Azure NetApp Files volumes | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/cross-region-replication-introduction)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-6/anf-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-7 - Azure NetApp Files ボリュームのゾーン間レプリケーションを有効にします

**Category: Availability**

**Impact: High**

**Guidance**

クロスゾーンレプリケーション (CZR) 機能は、異なる可用性ゾーン内のボリューム間のデータ保護を提供します。ある可用性ゾーンの Azure NetApp Files ボリューム (ソース) から、別の可用性の別の Azure NetApp Files ボリューム (宛先) にデータを非同期的にレプリケートできます。この機能により、ゾーン全体の停止や災害が発生した場合に、重要なアプリケーションをフェイルオーバーできます。

注: ボリュームは、クロスゾーンレプリケーション (CZR) またはクロスリージョンレプリケーション (CRR) を介してレプリケートできますが、両方を同時にレプリケートすることはできません。

**Resources**

- [Cross-zone replication of Azure NetApp Files volumes | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/cross-zone-replication-introduction)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-7/anf-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-8 - Azure NetApp Files メトリックを監視して、使用パターンとパフォーマンスの理解を深めます

**Category: Monitoring**

**Impact: Medium**

**Guidance**

Azure NetApp Files では、割り当てられたストレージ、実際のストレージ使用量、ボリューム IOPS、待機時間に関するメトリックが提供されます。これらの指標を使用すると、ネットアップアカウントの使用パターンとボリュームパフォーマンスをより深く理解できます。

**Resources**

- [Ways to monitor Azure NetApp Files | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/monitor-azure-netapp-files)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-8/anf-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-9 - Azure Policy を使用して組織の標準を適用し、Azure NetApp Files でコンプライアンスを大規模に評価します

**Category: Governance**

**Impact: Medium**

**Guidance**

Azure NetApp Files では、Azure Policy がサポートされています。Azure NetApp Files を Azure Policy と統合するには、組み込みのポリシー定義を使用するか、カスタム ポリシー定義を作成します。

**Resources**

- [Azure Policy definitions for Azure NetApp Files | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/azure-policy-definitions)
- [Creating custom policy definitions | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/governance/policy/tutorials/create-custom-policy-definition)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-9/anf-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-10 - Azure NetApp Files ボリュームへの既定のアクセスを制限します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

委任されたサブネットへのアクセスは、可能な限り特定の Azure Virtual Network にのみ付与する必要があります。
SMB 対応ボリュームの共有アクセス許可は、既定の [すべてのユーザー - フル コントロール] から制限する必要があります。
NFS対応ボリュームへのアクセスは、エクスポートポリシーやNFSv4.1 ACLを使用して制限する必要があります。
マウントパスの変更権限は、さらに制限する必要があります。


**Resources**

- [Configure network features for an Azure NetApp Files volume](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/configure-network-features)
- [Manage SMB share ACLs in Azure NetApp Files](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/manage-smb-share-access-control-lists)
- [Configure export policy for NFS or dual-protocol volumes](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/azure-netapp-files-configure-export-policy)
- [Configure access control lists on NFSv4.1 volumes for Azure NetApp Files](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/configure-access-control-lists)
- [Configure Unix permissions and change ownership mode for NFS and dual-protocol volumes](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/configure-unix-permissions-change-ownership-mode)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-10/anf-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-11 - サポートされているアプリケーションでSMBの継続的な可用性を活用します

**Category: Application Resilience**

**Impact: Medium**

**Guidance**

特定の SMB ベースのアプリケーションでは、SMB 透過フェールオーバーが必要です。SMB 透過フェールオーバーを使用すると、SMB ボリュームにデータを格納してアクセスするサーバー アプリケーションへの接続を中断することなく、Azure NetApp Files サービスでのメンテナンス操作が可能になります。特定のアプリケーションに対して SMB 透過フェールオーバーをサポートするために、Azure NetApp Files では SMB 継続的可用性共有オプションがサポートされています。

次の SMB ベースのアプリケーションには、継続的可用性オプションの使用を検討してください。
- Citrix App Layering
- FSLogix ユーザー プロファイル コンテナー
- FSLogix ODFC コンテナー
- Microsoft SQL Serverの
- MSIX アプリのアタッチ

**Resources**

- [Do I need to take special precautions for SMB-based applications? | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/faq-application-resilience#do-i-need-to-take-special-precautions-for-smb-based-applications)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-11/anf-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-12 - サービスメンテナンスイベントに対するアプリケーションの回復力を確保します

**Category: Application Resilience**

**Impact: Medium**

**Guidance**

Azure NetApp Files では、計画的なメンテナンス (プラットフォームの更新、サービスやソフトウェアのアップグレードなど) が時折行われる場合があります。そのため、ストレージ サービスのメンテナンス イベントに対処するためのアプリケーションの回復性設定を認識していることを確認してください。

**Resources**

- [What do you recommend for handling potential application disruptions due to storage service maintenance events? | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/faq-application-resilience#what-do-you-recommend-for-handling-potential-application-disruptions-due-to-storage-service-maintenance-events)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-12/anf-12.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
