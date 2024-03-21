+++
title = "Azure Virtual Desktop"
description = "Best practices and resiliency recommendations for Azure Virtual Desktop and associated resources and settings."
date = "1/9/24"
author = "yshafner"
msAuthor = "yonahshafner"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure Virtual Desktop and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                              |     Category      |  Impact  |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:--------:|:-------:|:-------------------:|
| [AVD-1 Use Private link when connecting to File Share or Key Vault](#avd-1---use-private-link-when-connecting-to-file-share-or-key-vault)                                   | Access & Security | Medium | Preview |         Yes         |
| [AVD-2 Monitor Service Health and Resource Health of AVD](#avd-2---monitor-service-health-and-resource-health-of-avd)                                                       |    Monitoring     |  Medium  | Preview |         No          |
| [AVD-3 Deploy Session Hosts in an Availability Zone](#avd-3---deploy-session-hosts-in-an-availability-zone)                                                                 |   Availability    |   High   | Preview |         No          |
| [AVD-4 Deploy Domain Controllers and DNS Servers in Azure Virtual Network Across Availability Zones](#avd-4---deploy-domain-controllers-and-dns-servers-in-azure-virtual-network-across-availability-zones) |   Availability    |  Medium  | Preview |         No          |
| [AVD-5 Implement RDP Shortpath for Public or Managed Networks](#avd-5---implement-rdp-shortpath-for-public-or-managed-networks)                                             |    Networking     |  Medium  | Preview |         No          |
| [AVD-6 Implement a Multi-Region BCDR Plan](#avd-6---implement-a-multi-region-bcdr-plan)                                                                                     | Disaster Recovery |  Medium  | Preview |         No          |
| [AVD-7 Store Golden Image Redundantly for Disaster Recovery](#avd-7---store-golden-image-redundantly-for-disaster-recovery)                                                 | Disaster Recovery |   Low    | Preview |         No          |
| [AVD-8 Capacity Planning for AVD Resources](#avd-8---capacity-planning-for-avd-resources)                                                                                   | Disaster Recovery |   Low    | Preview |         No          |
| [AVD-9 Ensure that FSLogix Storage Account is Redundant](#avd-9---ensure-that-fslogix-storage-account-is-redundant)                                                         |   Availability    |   High   | Preview |         No          |
| [AVD-10 Enable Azure Backup for FSLogix Storage Account](#avd-10---enable-azure-backup-for-fslogix-storage-account)                                                         |   Disaster Recovery    |   Medium   | Preview |         No          |
| [IT-2 - Replicate your Image Templates to a secondary region](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/compute/image-templates/#it-2---replicate-your-image-templates-to-a-secondary-region) | Disaster Recovery |  Low   | Preview |         Yes         |
| [CG-2 - Zone redundant storage should be used for image versions](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/compute/compute-gallery/#cg-2---zone-redundant-storage-should-be-used-for-image-versions)   | Availability | Medium | Preview |         Yes         |
| [VM-2 - Deploy VMs across Availability Zones](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-2---deploy-vms-across-availability-zones) | Availability | High | Verified | Yes |
| [VM-7 - Enable Backups on your VMs](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-7---backup-vms-with-azure-backup-service) | Disaster Recovery | Medium | Verified | Yes |
| [VM-8 - Production VMs should be using SSD disks](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-8---production-vms-should-be-using-ssd-disks) | System Efficiency | High | Verified | Yes |
| [ERC-1 - Connect your on-premises network to critical workloads in Azure through two or more ExpressRoute circuits in different peering locations](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/networking/expressroute-circuits/#erc-1---connect-your-on-premises-network-to-critical-workloads-in-azure-through-two-or-more-expressroute-circuits-in-different-peering-locations) |   Availability    |  High  | Preview |         No          |
| [ERC-2 - Ensure the two physical links of your ExpressRoute circuit are connected to two distinct edge devices in your network](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/networking/expressroute-circuits/#erc-2---ensure-the-two-physical-links-of-your-expressroute-circuit-are-connected-to-two-distinct-edge-devices-in-your-network)   |   Availability    |  High  | Preview |         No          |
| [VPNG-1 - Choose a Zone-redundant gateway](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/networking/vpn-gateway/#vpng-1---choose-a-zone-redundant-gateway)     |   Availability    |  High  | Preview |         Yes         |
| [VPNG-3 - Plan for Site-to-Site VPN and Azure ExpressRoute coexisting connection](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/networking/vpn-gateway/#vpng-3---plan-for-site-to-site-vpn-and-azure-expressroute-coexisting-connection) | Disaster Recovery |  High  | Preview |         No          |
| [NSG-4 - Configure NSG Flow Logs](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/networking/network-security-group/#nsg-4---configure-nsg-flow-logs)        | Monitoring        |  Medium   | Preview    |     Yes         |
| [VM-21 - Configure diagnostic settings for all Azure Virtual Machines](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-21---configure-diagnostic-settings-for-all-azure-virtual-machines) | Monitoring | Low | Preview | Yes |
| [VM-25 - Do not create more than 2000 Citrix VDA servers per subscription](https://yahanda.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-25---do-not-create-more-than-2000-citrix-vda-servers-per-subscription) | Application Resiliency | High | Preview | Yes |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### AVD-1 - ファイル共有または Key Vault に接続するときにプライベート リンクを使用します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

Private Link は、Azure Files や Key Vault など、Azure Virtual Desktop と連携して動作する他の Azure サービスで使用できます。回復性の観点から、これらのサービスにプライベート エンドポイントを実装して、待機時間、パケット損失、ダウンタイムなどの潜在的なインターネット関連の問題にさらされる可能性を減らすことをお勧めします。これにより、AVD と依存サービス間の通信の信頼性が高まります。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/networking#private-endpoints-private-link)
- [Private link](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/networking#private-endpoints-private-link)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-1/avd-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-2 - AVD のサービスの正常性とリソースの正常性を監視します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

Service Health を使用して、可用性を保証するために使用する Azure サービスとリージョンの正常性について常に情報を入手します。
サービスの問題、計画メンテナンス、または Azure Virtual Desktop リソースに影響を与える可能性のあるその他の変更を常に把握できるように、Service Health アラートを設定します。
Resource Health を使用して、VM とストレージ ソリューションを監視します。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/monitoring#resource-health)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-2/avd-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-3 - 可用性ゾーンでのセッションホストのデプロイ

**Category: Availability**

**Impact: High**

**Guidance**

可用性ゾーンまたは可用性セットにセッション ホストをデプロイすると、環境を停止から保護するのに役立ちます。

レイテンシーとインパクトの信頼性を最小限に抑え、データの同期を維持し、停止から保護することで、信頼性を高めます。1 つのゾーンで障害が発生した場合、リージョンのサービス、容量、高可用性は残りのゾーンでサポートされます。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/application-delivery#session-host-settings)
- [Availability Zones](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/application-delivery#session-host-settings)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-3/avd-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-4 - 可用性ゾーン間で Azure Virtual Network にドメイン コントローラーと DNS サーバーをデプロイします

**Category: Availability**

**Impact: Medium**

**Guidance**

AVD で AD DS ID ソリューションを使用する場合は、可用性ゾーン間で Azure 仮想マシンにドメイン コントローラーと DNS サーバーをデプロイすることをお勧めします。これにより、オンプレミス サービスへの依存関係がなくなることで環境の信頼性が向上し、ユーザー認証のパスが短くなることでパフォーマンスが向上します。

この推奨事項は、ID プロバイダーとして Microsoft Entra を利用している場合には関係ありません。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/identity/adds-extend-domain#reliability)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-4/avd-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-5 - パブリックネットワークまたはマネージドネットワークにRDPショートパスを実装します

**Category: Networking**

**Impact: Medium**

**Guidance**

AVD の RDP Shortpath を有効にすることをお勧めします。RDP Shortpath は、サポートされている Windows リモート デスクトップ クライアントとセッション ホストの間に直接 UDP ベースのトランスポートを確立する Azure Virtual Desktop の機能です。既定では、リモート デスクトップ プロトコル (RDP) は UDP を使用して接続を確立しようとし、フォールバック接続メカニズムとして TCP ベースの逆接続トランスポートを使用します。TCP ベースの逆接続トランスポートは、さまざまなネットワーク構成と最高の互換性を提供し、RDP 接続を確立するための高い成功率を備えています。UDPベースのトランスポートは、接続の信頼性が高く、遅延の一貫性が高くなります。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/rdp-shortpath?tabs=managed-networks)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-5/avd-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-6 - 複数リージョンの BCDR 計画を実装します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

AVD にはマルチリージョン デプロイ(アクティブ/アクティブ)を採用することをお勧めします。各リージョンには、プライマリ リージョンが停止した場合に備えて、少なくとも ID、名前解決、AVD 管理リソース、およびセッション ホストが含まれている必要があります。

**Resources**

- [Multi-region BCDR](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/wvd/azure-virtual-desktop-multi-region-bcdr)
- [Learn More](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/business-continuity#active-active-scenarios)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-6/avd-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-7 - ディザスタリカバリのためにゴールデンイメージを冗長的に保存します

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

完全な BCDR 戦略が設定されていない場合は、ゾーン冗長ストレージを使用して、可用性ゾーン間でゴールド イメージを格納することを検討してください。イメージを利用できるようにしておくと、ゾーンまたはリージョンの停止が発生した場合に、より迅速な回復が可能になります。

**Resources**

- [Golden Image](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/business-continuity#golden-images)
- [Learn More](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/application-delivery#fault-tolerance)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-7/avd-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-8 - AVD リソースのキャパシティ プランニングを行います

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

サブスクリプションの制限を監視して計画します。Azure Virtual Desktop のデプロイを綿密に監視し、サブスクリプション内のリソースの使用状況を追跡します。容量をプロアクティブに監視することで、潜在的な課題を早期に特定し、制限に達しないように適切なアクションを実行できます。
さらにスケーリングが必要な場合は、複数のサブスクリプションにまたがってスケーリングすることを検討するか、Azure サポートと連携して、ビジネス要件に基づいて制限を調整してください。
多数のユーザーを処理するには、複数のホスト プールを作成して水平方向にスケーリングすることを検討してください。

**Resources**

- [Capacity Planning](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/business-continuity#capacity-planning)
- [Learn More](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/wvd/windows-virtual-desktop#azure-virtual-desktop-limitations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-8/avd-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-9 - FSLogix ストレージ アカウントが冗長であることを確認します

**Category: Availability**

**Impact: High**

**Guidance**

FSLogix を使用するときは、ユーザー プロファイルの冗長性を確保することが重要です。AVD で FSLogix を使用する場合、ストレージ アカウント内のファイル共有にデプロイされます。Azure Storage アカウント内のデータは、常にプライマリ リージョンで 3 回レプリケートされます。以下は、プライマリまたはペアのリージョンでデータをレプリケートする方法のオプションです。
LRS は、レプリケーションのコストが最も低くなります (高可用性と持続性を備えたアプリには推奨されません)。

- LRS は イレブン ナイン の耐久性を提供し、1 つの物理的な場所で 3 回複製します。
- ZRS は、ゾーン間で高可用性を必要とするアプリに推奨されます。ZRS は トゥエルブ ナイン の耐久性を提供します。3 つの可用性ゾーン間でレプリケートされる
- GRS は、さらに 3 つのコピーをセカンダリ領域にレプリケートし、シックスティーン ナイン の耐久性を提供します。
- GZRS は、geo レプリケーション全体で高可用性と冗長性の両方を提供します。1年間で シックスティーン ナイン の耐久性を提供します。

一般的には、できるだけ安全で冗長なデータを保存することをお勧めします。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/storage#user-profiles)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-9/avd-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-10 - FSLogix ストレージ アカウントの Azure Backup を有効にします

**Category: Backup/Storage**

**Impact: Medium**

**Guidance**

FSLogix ストレージ アカウントでバックアップを有効にすることをお勧めします。ユーザー プロファイルの回復性を確保することで、停止中もユーザー データとエクスペリエンスの一貫性を保つことができます。

**Resources**

- [FSLogix](https://learn.microsoft.com/ja-jp/fslogix/overview-what-is-fslogix)
- [Backup Storage Account](https://learn.microsoft.com/ja-jp/azure/backup/blob-backup-configure-manage?tabs=operational-backup)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-10/avd-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
