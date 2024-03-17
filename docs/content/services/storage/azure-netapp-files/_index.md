+++
title = "Azure NetApp Files"
description = "Best practices and resiliency recommendations for Azure NetApp Files and associated resources and settings."
date = "8/30/23"
author = "maheshbenke"
msAuthor = "maheshbenke"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure NetApp Files and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State   | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------: | :-----------------: |
| [ANF-1 - Use the correct service level and volume quota size for the expected performance level](#anf-1---use-the-correct-service-level-and-volume-quota-size-for-the-expected-performance-level) | System Efficiency | High | Preview  |         No         |
| [ANF-2 - Use standard network features for production in Azure NetApp Files](#anf-2---use-standard-network-features-for-production-in-azure-netapp-files) | Networking | High | Preview  |         Yes         |
| [ANF-3 - Use availability zones for high availability in Azure NetApp Files](#anf-3---use-availability-zones-for-high-availability-in-azure-netapp-files) | Availability | High | Preview  |         Yes         |
| [ANF-4 - Use snapshot and backup for in-region data protection in Azure NetApp Files](#anf-4---use-snapshot-and-backup-for-in-region-data-protection-in-azure-netapp-files) | Availability | High | Preview  |         No         |
| [ANF-5 - Enable Cross-region replication of Azure NetApp Files volumes](#anf-5---enable-cross-region-replication-of-azure-netapp-files-volumes) | Disaster Recovery | High | Preview  |         Yes         |
| [ANF-6 - Enable Cross-zone replication of Azure NetApp Files volumes](#anf-6---enable-cross-zone-replication-of-azure-netapp-files-volumes) | Availability | High | Preview  |         Yes         |
| [ANF-7 - Monitor Azure NetApp Files metrics to better understand usage pattern and performance](#anf-7---monitor-azure-netapp-files-metrics-to-better-understand-usage-pattern-and-performance) | Monitoring | Medium | Preview  |         No         |
| [ANF-8 - Use Azure policy to enforce organizational standards and to assess compliance at-scale in Azure NetApp Files](#anf-8---use-azure-policy-to-enforce-organizational-standards-and-to-assess-compliance-at-scale-in-azure-netapp-files) | Governance | Medium | Preview  |         No         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ANF-1 - 予想されるパフォーマンスレベルに対して正しいサービスレベルとボリュームクォータサイズを使用します

**Category: System Efficiency**

**Impact: High**

**Guidance**

サービス レベルは、容量プールの属性です。サービス レベルは、ボリュームに割り当てられているクォータに基づいて、容量プール内のボリュームに対して許容される最大スループットによって定義され、区別されます。スループットは、読み取り速度と書き込み速度の組み合わせです。Azure NetApp Files では、次の 3 つのサービス レベルがサポートされています。

- 標準 (1TiB あたり 16 MiB/秒) スループット
- Premium (1TiB あたり 64 MiB/秒) スループット
- Ultra (1TiB あたり 128 MiB/秒) スループット

**Resources**

- [Service levels for Azure NetApp Files | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/azure-netapp-files-service-levels)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-1/anf-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-2 - Azure NetApp Files での運用環境に Standard ネットワーク機能を使用します

**Category: Networking**

**Impact: High**

**Guidance**

標準ネットワーク機能を使用すると、より高い IP 制限と、委任されたサブネット上のネットワーク セキュリティ グループとユーザー定義ルート、追加の接続パターンなどの標準 VNet 機能が可能になります。
標準ネットワーク機能の対応地域は [こちら](https://docs.microsoft.com/ja-jp/azure/azure-netapp-files/azure-netapp-files-network-topologies#supported-regions-for-standard-network-feature) を参照ください。

**Resources**

- [Guidelines for Azure NetApp Files network planning | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/azure-netapp-files-network-topologies)

**Resource Graph Query/Scripts**

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

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-3/anf-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-4 - Azure NetApp Files でリージョン内データ保護にスナップショットとバックアップを使用します

**Category: Availability**

**Impact: High**

**Guidance**

Azure NetApp Files スナップショット テクノロジは、パフォーマンスに影響を与えることなく、安定性、スケーラビリティ、迅速な復旧性を提供します。
Azure NetApp Files は、長期的な復旧、アーカイブ、コンプライアンスのためのフル マネージド バックアップ ソリューションをサポートしています。バックアップは、バックアップと同じリージョン内の新しいボリュームに復元できます。Azure NetApp Files によって作成されたバックアップは、短期的な復旧や複製に使用できるボリューム スナップショットとは無関係に、Azure Storage に格納されます。

**Resources**

- [Snapshots](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/data-protection-disaster-recovery-options#snapshots)
- [Backup](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/data-protection-disaster-recovery-options#backups)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-4/anf-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-5 - Azure NetApp Files ボリュームのリージョン間レプリケーションを有効にします

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Azure NetApp Files レプリケーション機能は、リージョン間ボリューム レプリケーションによるデータ保護を提供します。あるリージョンの Azure NetApp Files ボリューム (ソース) から、別のリージョンの別の Azure NetApp Files ボリューム (宛先) にデータを非同期的にレプリケートできます。この機能により、リージョン全体の停止や災害が発生した場合に、重要なアプリケーションをフェールオーバーできます。

**Resources**

- [Cross-zone replication of Azure NetApp Files volumes | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/cross-region-replication-introduction)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-5/anf-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-6 - Azure NetApp Files ボリュームのクロスゾーン レプリケーションを有効にします

**Category: Availability**

**Impact: High**

**Guidance**

クロスゾーンレプリケーション (CZR) 機能は、異なる可用性ゾーン内のボリューム間のデータ保護を提供します。ある可用性ゾーンの Azure NetApp Files ボリューム (ソース) から、別の可用性の別の Azure NetApp Files ボリューム (宛先) にデータを非同期的にレプリケートできます。この機能により、ゾーン全体の停止や災害が発生した場合に、重要なアプリケーションをフェイルオーバーできます。

**Resources**

- [Cross-zone replication of Azure NetApp Files volumes | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/cross-zone-replication-introduction)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-6/anf-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-7 - Azure NetApp Files メトリックを監視して、使用パターンとパフォーマンスをよりよく理解します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

Azure NetApp Files では、割り当てられたストレージ、実際のストレージ使用量、ボリューム IOPS、待機時間に関するメトリックが提供されます。これらの指標を使用すると、ネットアップアカウントの使用パターンとボリュームパフォーマンスをより深く理解できます。

**Resources**

- [Ways to monitor Azure NetApp Files | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/monitor-azure-netapp-files)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-7/anf-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ANF-8 - Azure Policy を使用して組織標準を適用し、Azure NetApp Files でコンプライアンスを大規模に評価します

**Category: Governance**

**Impact: Medium**

**Guidance**

Azure NetApp Files は Azure Policy をサポートしています。Azure NetApp Files と Azure Policy を統合するには、[カスタム ポリシー定義の作成](https://learn.microsoft.com/ja-jp/azure/governance/policy/tutorials/create-custom-policy-definition)。例については、[Azure Policy を使用してスナップショット ポリシーを適用する](https://anfcommunity.com/2021/08/30/enforce-snapshot-policies-with-azure-policy/) と [Azure NetApp Files で Azure Policy を使用できるようになりました](https://anfcommunity.com/2021/04/19/azure-policy-now-available-for-azure-netapp-files/) を参照してください。

**Resources**

- [Azure Policy definitions for Azure NetApp Files | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/azure-netapp-files/azure-policy-definitions)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/anf-8/anf-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
