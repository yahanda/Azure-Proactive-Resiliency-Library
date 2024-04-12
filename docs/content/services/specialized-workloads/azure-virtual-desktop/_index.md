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
| Recommendation | Category | Impact | State | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:--------:|:-------:|:-------------------:|
| [AVD-1 - Use Private link when connecting to File Share or Key Vault](#avd-1---use-private-link-when-connecting-to-file-share-or-key-vault) | Access & Security | Medium | Verified | No |
| [AVD-2 - Monitor Service Health and Resource Health of AVD](#avd-2---monitor-service-health-and-resource-health-of-avd) | Monitoring | High | Verified | No |
| [AVD-4 - Deploy Domain Controllers and DNS Servers in Azure Virtual Network Across Availability Zones](#avd-4---deploy-domain-controllers-and-dns-servers-in-azure-virtual-network-across-availability-zones) | Availability | High | Verified | No |
| [AVD-5 - Implement RDP Shortpath for Public or Managed Networks](#avd-5---implement-rdp-shortpath-for-public-or-managed-networks) | Networking | Medium | Verified | No |
| [AVD-6 - Implement a Multi-Region BCDR Plan](#avd-6---implement-a-multi-region-bcdr-plan) | Disaster Recovery | Medium | Verified | No |
| [AVD-7 - Store Golden Image Redundantly for Disaster Recovery](#avd-7---store-golden-image-redundantly-for-disaster-recovery) | Disaster Recovery | Low | Verified | No |
| [AVD-8 - Capacity Planning for AVD Resources](#avd-8---capacity-planning-for-avd-resources) | Disaster Recovery | Low | Verified | No |
| [AVD-9 - Ensure that FSLogix Storage Account is Redundant](#avd-9---ensure-that-fslogix-storage-account-is-redundant) | Availability | High | Verified | Yes |
| [AVD-10 - Enable Azure Backup for FSLogix Storage Account](#avd-10---enable-azure-backup-for-fslogix-storage-account) | Storage | Medium | Verified | No |
| [AVD-11 - Scaling plans should be created per region and not scaled across regions](#avd-11---scaling-plans-should-be-created-per-region-and-not-scaled-across-regions) | Disaster Recovery | Medium | Verified | No |
| [AVD-13 - Validate that the AVD session hosts can communicate with the AVD control plane and UDP ports are open if UDP is in use](#avd-13---validate-avd-session-host-connectivity-to-the-avd-control-plane-and-udp-ports-open-if-in-use) | Networking | Medium | Verified | No |
| [AVD-14 - Ensure Secondary Entra ID connect synchronization server](#avd-14---ensure-secondary-entra-id-connect-synchronization-server) | Access & Security | Low | Verified | No |
| [AVD-15 - Deploy paired Domain Controllers in the same region as AVD session hosts](#avd-15---deploy-paired-domain-controllers-in-the-same-region-as-avd-session-hosts) | Disaster Recovery | High | Verified | No |
| [AVD-16 - Ensure DNS regions are replicated to avoid single point of failure](#avd-16---ensure-dns-regions-are-replicated-to-avoid-single-point-of-failure) | Networking | Medium | Verified | No |
| [AVD-17 - Capacity Planning for AVD Resources](#avd-17---capacity-planning-for-avd-resources) | Disaster Recovery | Low | Verified | No |
| [AVD-18 - Create new version of updated image and replace session hosts rather than update host directly](#avd-18---create-updated-image-version-and-replace-session-hosts-rather-than-updating-host-directly) | Governance | Low | Verified | No |
| [AVD-19 - Pooled Create a validation pool for testing of planned updates](#avd-19---pooled-create-a-validation-pool-for-testing-of-planned-updates) | Governance | Medium | Verified | No |
| [AVD-20 - Pooled Configure scheduled agent updates](#avd-20---pooled-configure-scheduled-agent-updates) | System Efficiency | Medium | Verified | No |
| [AVD-21 - Personal Create a validation pool for testing of planned updates](#avd-21---personal-create-a-validation-pool-for-testing-of-planned-updates) | Governance | Low | Verified | No |
| [AVD-22 - Use Azure Site Recovery or Backups on VMs supporting personal desktops](#avd-22---use-azure-site-recovery-or-backups-on-vms-supporting-personal-desktops) | Disaster Recovery | Medium | Verified | No |
| [AVD-23 - Ensure a unique OU when deploying VMs to Domain](#avd-23---ensure-a-unique-ou-when-deploying-vms-to-domain) | Governance | Medium | Verified | No |
| [AVD-24 - Ensure the standard FSLogix configuration is deployed](#avd-24---ensure-the-standard-fslogix-configuration-is-deployed) | Storage | Medium | Verified | No |
| [AVD-25 - Ensure user permissions are set correctly on SMB shares](#avd-25---ensure-user-permissions-are-set-correctly-on-smb-shares) | Storage | Medium | Verified | No |
| [AVD-26 - Configure Diagnostic Settings for FSLogix logs and enable review for accounts](#avd-26---configure-diagnostic-settings-for-fslogix-logs-and-enable-review-for-accounts) | Storage | Medium | Verified | No |
| [AVD-27 - Manually update new FSLogix image when available](#avd-27---manually-update-new-fslogix-image-when-available) | Availability | Low | Verified | No |
| [AVD-28 - Turn on Continuous Availability for ANF if using App Attach](#avd-28---turn-on-continuous-availability-for-anf-if-using-app-attach) | App Attach Storage | Medium | Verified | No |
| [AVD-29 - App attach should be placed in separate file share; Disaster recovery plan should include App attach storage](#avd-29---app-attach-should-be-placed-in-separate-file-share-and-disaster-recovery-plan-should-include-app-attach-storage) | Storage | Medium | Verified | No |
| [AVD-30 - Ensure virtual networks have route tables/route server configured for all regions](#avd-30---ensure-virtual-networks-have-route-tablesroute-server-configured-for-all-regions) | Networking | Medium | Verified | No |
| [AVD-31 - Ensure virtual networks isolation with separate IP space and NSGs for Prod and DR](#avd-31---ensure-virtual-networks-isolation-with-separate-ip-space-and-nsgs-for-prod-and-dr) | Networking | Medium | Verified | No |
| [AVD-33 - Ensure route tables accommodate failover](#avd-33---ensure-route-tables-accommodate-failover) | Disaster Recovery | Medium | Verified | No |
| [AVD-34 - Ensure Resilient Deployment of Keyvault for AVD Host Pools](#avd-34---provision-secondary-key-vault-for-disaster-recovery) | Disaster Recovery | High | Verified | No |
| [AVD-35 - Configure AVD insights Workbook](#avd-35---configure-avd-insights-workbook) | Monitoring | High | Verified | No |
| [AVD-36 - Ensure separate log analytics workspaces for Prod and DR](#avd-36---ensure-separate-log-analytics-workspaces-for-prod-and-dr) | Disaster Recovery | Low | Verified | No |
| [AVD-37 - Organize AVD resources using the AVD Scale unit model described by the AVD Landing Zone Methodology](#avd-37---organize-avd-resources-using-the-avd-scale-unit-model-described-by-the-avd-landing-zone-methodology) | Governance | Low | Verified | No |
| [IT-2 - Replicate your Image Templates to a secondary region](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/image-templates/#it-2---replicate-your-image-templates-to-a-secondary-region) | Disaster Recovery | Low | Preview | Yes |
| [CG-2 - Zone redundant storage should be used for image versions](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/compute-gallery/#cg-2---zone-redundant-storage-should-be-used-for-image-versions) | Availability | Medium | Verified | Yes |
| [VM-2 - Deploy VMs across Availability Zones](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-2---deploy-vms-across-availability-zones) | Availability | High | Verified | Yes |
| [VM-7 - Enable Backups on your VMs](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-7---backup-vms-with-azure-backup-service) | Disaster Recovery | Medium | Verified | Yes |
| [VM-8 - Production VMs should be using SSD disks](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-8---production-vms-should-be-using-ssd-disks) | System Efficiency | High | Verified | Yes |
| [VM-21 - Configure diagnostic settings for all Azure Virtual Machines](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-21---configure-diagnostic-settings-for-all-azure-virtual-machines) | Monitoring | Low | Preview | Yes |
| [ERC-1 - Connect your on-premises network to critical workloads in Azure through two or more ExpressRoute circuits in different peering locations](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/expressroute-circuits/#erc-1---connect-your-on-premises-network-to-critical-workloads-in-azure-through-two-or-more-expressroute-circuits-in-different-peering-locations) | Availability | High | Verified | No |
| [ERC-2 - Ensure the two physical links of your ExpressRoute circuit are connected to two distinct edge devices in your network](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/expressroute-circuits/#erc-2---ensure-the-two-physical-links-of-your-expressroute-circuit-are-connected-to-two-distinct-edge-devices-in-your-network) | Availability | High | Verified | No |
| [VPNG-1 - Choose a Zone-redundant gateway](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/vpn-gateway/#vpng-1---choose-a-zone-redundant-gateway) | Availability | High | Verified | Yes |
| [NSG-4 - Configure NSG Flow Logs](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/network-security-group/#nsg-4---configure-nsg-flow-logs) | Monitoring | Medium | Preview | Yes |
| [ST-1 - Ensure that Storage Account configuration is at least Zone redundant](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/storage/storage-account/#st-1---ensure-that-storage-account-configuration-is-at-least-zone-redundant) | Storage | High | Verified | Yes |
| [WADS-3 - Ensure that all fault-points and fault-modes are understood and operationalized](https://azure.github.io/Azure-Proactive-Resiliency-Library/well-architected/2-design/#wads-3---ensure-that-all-fault-points-and-fault-modes-are-understood-and-operationalized) | Availability | High | Verified | No |
| [WADS-7 - Design a BCDR strategy that will help to meet the business requirements](https://azure.github.io/Azure-Proactive-Resiliency-Library/well-architected/2-design/#wads-7---design-a-bcdr-strategy-that-will-help-to-meet-the-business-requirements) | Disaster Recovery | High | Verified | No |

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

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-1/avd-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-2 - AVD のサービスの正常性とリソースの正常性を監視します

**Category: Monitoring**

**Impact: High**

**Guidance**

Service Health を使用して、可用性を保証するために使用する Azure サービスとリージョンの正常性について常に情報を入手します。
サービスの問題、計画メンテナンス、または Azure Virtual Desktop リソースに影響を与える可能性のあるその他の変更を常に把握できるように、Service Health アラートを設定します。
Resource Health を使用して、VM とストレージ ソリューションを監視します。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/monitoring#resource-health)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-2/avd-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-4 - 可用性ゾーン間で Azure Virtual Network にドメイン コントローラーと DNS サーバーをデプロイします

**Category: Availability**

**Impact: High**

**Guidance**

AVD で AD DS ID ソリューションを使用する場合は、可用性ゾーン間で Azure 仮想マシンにドメイン コントローラーと DNS サーバーをデプロイすることをお勧めします。これにより、オンプレミス サービスへの依存関係がなくなることで環境の信頼性が向上し、ユーザー認証のパスが短くなることでパフォーマンスが向上します。

この推奨事項は、ID プロバイダーとして Microsoft Entra を利用している場合には関係ありません。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/identity/adds-extend-domain#reliability)

**Resource Graph Query**

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

**Resource Graph Query**

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

**Resource Graph Query**

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

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-7/avd-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-8 - AVD リソースのキャパシティ プランニングを行います

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

サブスクリプションの制限と API の調整制限を監視して計画します。Azure Virtual Desktop のデプロイを綿密に監視し、サブスクリプション内のリソースの使用状況を追跡します。容量をプロアクティブに監視することで、潜在的な課題を早期に特定し、制限に達しないように適切なアクションを実行できます。
さらにスケーリングが必要な場合は、複数のサブスクリプションにまたがってスケーリングすることを検討するか、Azure サポートと連携して、ビジネス要件に基づいて制限を調整してください。
多数のユーザーを処理するには、複数のホスト プールを作成して水平方向にスケーリングすることを検討してください。

**Resources**

- [Capacity Planning](https://learn.microsoft.com/ja-jp/azure/well-architected/azure-virtual-desktop/business-continuity#capacity-planning)
- [Learn More](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/wvd/windows-virtual-desktop#azure-virtual-desktop-limitations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-8/avd-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-9 - FSLogix ストレージ アカウントが冗長であることを確認します

**Category: Availability**

**Impact: Medium**

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

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-9/avd-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-10 - FSLogix ストレージ アカウントの Azure Backup を有効にします

**Category: Storage**

**Impact: Medium**

**Guidance**

FSLogix ストレージ アカウントでバックアップを有効にすることをお勧めします。ユーザー プロファイルの回復性を確保することで、停止中もユーザー データとエクスペリエンスの一貫性を保つことができます。

**Resources**

- [FSLogix](https://learn.microsoft.com/ja-jp/fslogix/overview-what-is-fslogix)
- [Backup Storage Account](https://learn.microsoft.com/ja-jp/azure/backup/blob-backup-configure-manage?tabs=operational-backup)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-10/avd-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-11 - スケーリング プランはリージョンごとに作成し、リージョン間でスケーリングしないでください

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance:**
各リージョンには、そのリージョン内のホスト プールに割り当てられた独自のスケーリング プランがあります。ただし、これらのプランは、リージョンで障害が発生した場合にアクセスできなくなる可能性があります。このリスクを軽減するには、別のリージョンにセカンダリ スケーリング プランを作成することをお勧めします。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/autoscale-scaling-plan?tabs=portal)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-11/avd-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-13 - AVD コントロール プレーンへの AVD セッション ホストの接続を検証し、使用中の場合は UDP ポートが開いていることを確認します

**Category: Networking**

**Impact: Medium**

**Guidance:**
AVD セッション ホストが AVD コントロール プレーンと効果的に通信できること、および UDP が使用されている場合は UDP ポートが開いていることを確認します。VM の AVD コントロール プレーンへの接続を検証し、UDP TURN ポートのアクセシビリティを確認します。グローバルURLをホワイトリストに登録し、UDP/TURNポートが開いてアクセス可能であることを確認して、スムーズなユーザー接続を促進します。適切な接続検証により、AVD 環境内で最適なパフォーマンスとユーザー エクスペリエンスが保証されます。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/troubleshoot-rdp-shortpath)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-13/avd-13.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-14 - セカンダリ Entra ID が同期サーバーに接続されていることを確認します

**Category: Access & Security**

**Impact: Low**

**Guidance:**
ハイブリッド - Entra ID Connect は Azure での実行に最適ですが、オンプレミスでホストできます。セカンダリ以上の VM は、フェールオーバーが発生した場合にステージング モードでセットアップする必要があります。
プライマリ サーバーが停止した場合に Entra と同期するために、Entra Connect のステージング モードでセカンダリ サーバーを設定します。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/entra/identity/hybrid/connect/how-to-connect-install-multiple-domains)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-14/avd-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-15 - ペアのドメイン コントローラを AVD セッション ホストと同じリージョンにデプロイします

**Category: Disaster Recovery**

**Impact: High**

**Guidance:**
セッション ホストを持つ各リージョンに、ID に関する高可用性をサポートするために、同じリージョンに複数のドメイン コントローラーがあることを確認します。
ハイブリッド シナリオでは、AVD セッション ホストを持つ各 Azure リージョンには、Azure に Active Directory ドメイン コントローラーがあり、リージョン内の回復性のために可用性ゾーンまたは可用性セットを使用する必要があります。これにより、ER/VPN/Azure 間の依存関係への依存関係も軽減されます。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/azure-virtual-desktop/azure-virtual-desktop-multi-region-bcdr)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-15/avd-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-16 - 単一障害点を回避するために DNS リージョンがレプリケートされていることを確認します

**Category: Networking**

**Impact: Medium**

**Guidance:**
Active Directory Domain Services (AD DS) に統合された DNS/その他は、マルチリージョン ゾーン間で第二/第三のお客様 DNS をターゲットにする必要があります。カスタム DNS を使用する場合は、単一障害点を回避するために、冗長な DNS サーバーがあることを確認します。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/azure-virtual-desktop/azure-virtual-desktop-multi-region-bcdr)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-16/avd-16.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-17 - AVD リソースのキャパシティ プランニングをします

**Category: Disaster Recovery**

**Impact: Low**

**Guidance:**
サブスクリプションの制限と API の調整制限を監視して計画します。Azure Virtual Desktop のデプロイを綿密に監視し、サブスクリプション内のリソースの使用状況を追跡します。容量をプロアクティブに監視することで、潜在的な課題を早期に特定し、制限に達しないように適切なアクションを実行できます。さらにスケーリングが必要な場合は、複数のサブスクリプションにまたがってスケーリングすることを検討するか、Azure サポートと連携して、ビジネス要件に基づいて制限を調整してください。多数のユーザーを処理するには、複数のホスト プールを作成して水平方向にスケーリングすることを検討してください。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/wvd/windows-virtual-desktop#azure-virtual-desktop-limitations)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-17/avd-17.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-18 - ホストを直接更新するのではなく、更新されたイメージ バージョンを作成し、セッション ホストを置き換えます

**Category: Governance**

**Impact: Low**

**Guidance:**
Azure Virtual Desktop 環境内でイメージの更新を処理するための体系的なプロセスを確立します。個々のセッション ホストを直接更新する代わりに、更新されたイメージの新しいバージョンを作成します。このプロセスには、必要な更新と設定を含むゴールド イメージの作成と設定が含まれます。新しいイメージの準備ができたら、既存のセッション ホストを、更新されたイメージを使用するインスタンスに置き換えます。このアプローチにより、すべてのセッション ホスト間で一貫性が確保され、構成のずれのリスクが最小限に抑えられます。さらに、更新で問題が発生した場合に、以前のイメージ バージョンにすばやくロールバックできます。このプロセスを実装すると、メンテナンス作業が合理化され、すべてのセッション ホストが最新の構成と更新プログラムで最新の状態に保たれます。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/training/modules/create-manage-session-host-image/)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-18/avd-18.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-19 - [Pooled] 計画された更新プログラムをテストするための検証プールを作成します

**Category: Governance**

**Impact: Medium**

**Guidance:**
少なくとも 1 つの検証プールで、AVD の計画された更新によって問題が発生した場合に早期に警告します。ビジネス要件に基づいて制限を調整するためのサポート。多数のユーザーを処理するには、複数のホスト プールを作成して水平方向にスケーリングすることを検討してください。
また、計画された更新プログラムをテストするためにホスト プールが定期的に使用されていることも確認します。
ホスト プールは、Azure Virtual Desktop 環境内の 1 つ以上の同一の仮想マシンのコレクションです。サービスの更新が最初に適用される検証ホスト プールを作成することを強くお勧めします。検証ホスト プールを使用すると、サービスが標準環境または非検証環境に適用する前に、サービスの更新を監視できます。検証ホスト プールがないと、エラーの原因となる変更が検出されず、標準環境のユーザーにダウンタイムが発生する可能性があります。
アプリが最新の更新プログラムで動作するようにするには、検証ホスト プールを、検証されていない環境のホスト プールとできるだけ類似させる必要があります。ユーザーは、標準ホスト プールに接続するのと同じ頻度で検証ホスト プールに接続する必要があります。ホスト プールで自動テストを行っている場合は、検証ホスト プールで自動テストを含める必要があります。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/configure-validation-environment?tabs=azure-portal)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-19/avd-19.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-20 - [Pooled] スケジュールされたエージェントの更新を構成します

**Category: System Efficiency**

**Impact: Medium**

**Guidance:**
AVD エージェントの更新のメンテナンス期間を提供するスケジュールが作成されていることを確認します。
スケジュールされたエージェントの更新機能を使用すると、Azure Virtual Desktop エージェント、サイド バイ サイド スタック、Geneva Monitoring エージェントのメンテナンス期間を最大 2 つ作成して更新し、営業時間のピーク時に更新が行われないようにすることができます。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/scheduled-agent-updates)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-20/avd-20.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-21 - [Personal] 計画された更新プログラムをテストするための検証プールを作成します

**Category: Governance**

**Impact: Low**

**Guidance:**
少なくとも 1 つの検証プールで、AVD の計画された更新によって問題が発生した場合に早期に警告します。また、計画された更新プログラムをテストするためにホスト プールが定期的に使用されていることも確認します。
ホスト プールは、Azure Virtual Desktop 環境内の 1 つ以上の同一の仮想マシンのコレクションです。サービスの更新が最初に適用される検証ホスト プールを作成することを強くお勧めします。検証ホスト プールを使用すると、サービスが標準環境または非検証環境に適用する前に、サービスの更新を監視できます。検証ホスト プールがないと、エラーの原因となる変更が検出されず、標準環境のユーザーにダウンタイムが発生する可能性があります。
アプリが最新の更新プログラムで動作するようにするには、検証ホスト プールを、検証されていない環境のホスト プールとできるだけ類似させる必要があります。ユーザーは、標準ホスト プールに接続するのと同じ頻度で検証ホスト プールに接続する必要があります。ホスト プールで自動テストを行っている場合は、検証ホスト プールで自動テストを含める必要があります。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/configure-validation-environment?tabs=azure-portal)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-21/avd-21.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-22 - 個人用デスクトップをサポートする VM で Azure Site Recovery またはバックアップを使用します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance:**
Azure Site Recovery (ASR) を活用するか、個人用ホスト プールに Azure Backup を実装してシームレスなフェールオーバーとフェールバック機能を実現し、個人用デスクトップをサポートする VM をセカンダリ Azure リージョンにレプリケーションできるようにします。これにより、災害や予期しない停止が発生した場合に、これらの VM が既知の状態から確実に復旧されます。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/scheduled-agent-updates)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-22/avd-22.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-23 - VM をドメインにデプロイするときに一意の OU を確保します

**Category: Governance**

**Impact: Medium**

**Guidance:**
ハイブリッド VM は、一意の OU 内に存在する必要があります。
AD に参加しているセッションを使用する場合、ホストは一意の OU を使用して、ホストプールごとに特定の AVD 構成をターゲットにすることでメリットが得られます。例としては、Fslogix、タイムアウト制限、セッション制御などがあります。また、運用と DR の組織単位をセグメント化して、リソースが環境ごとに構成されるようにすることも重要です。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm#configure-the-vms-and-install-active-directory-domain-services)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-23/avd-23.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-24 - 標準の FSLogix 構成がデプロイされていることを確認します

**Category: Storage**

**Impact: High**

**Guidance:**
すべてのセッション ホストに標準の FSLogix 構成がデプロイされていることを確認します。設定の一貫性とベストプラクティスとの整合性を定期的に検証します。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/fslogix/reference-configuration-settings?tabs=profiles)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-24/avd-24.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-25 - SMB共有でユーザー権限が正しく設定されていることを確認します

**Category: Storage**

**Impact: High**

**Guidance:**
SMB共有にユーザー権限が正しく設定されていることを確認して、ユーザーが自分のプロファイルにのみ適切なアクセス権を持ち、他のユーザープロファイルにはアクセスできず、管理者がルートボリュームにフルアクセスできるようにします。また、DR イベントが発生した場合に 2 次ストレージ・パスの権限が設定されていることを確認します。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/fslogix/how-to-configure-storage-permissions)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-25/avd-25.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-26 - FSLogix ログの診断設定を構成し、アカウントのレビューを有効にします

**Category: Storage**

**Impact: Medium**

**Guidance:**
FSLogix ログを定期的に確認して、ログインとプロファイルのマウントに関連するエラーと問題を確認します。イベントは、セッション ホスト内をローカルに確認し、Azure Monitor エージェントが使用されている場合は Log Analytics で確認することもできます。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/fslogix/troubleshooting-events-logs-diagnostics)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-26/avd-26.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-27 - 使用可能な場合は、新しい FSLogix イメージを手動で更新します

**Category: Governance**

**Impact: Low**

**Guidance:**
FSLogix エージェントのアップグレードを定期的にチェックし、FSLogix を最新の状態に維持するプロセスが整っていることを確認します。お客様には、デプロイ プロセスが許す限り迅速に最新バージョンの FSLogix にアップグレードすることをお勧めします。FSLogix は、お客様の展開に影響を与える現在および潜在的なバグに対処する修正プログラム リリースを提供します。さらに、これはサポートケースを開くときの最初の要件です。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/fslogix/how-to-install-fslogix)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-27/avd-27.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-28 - App Attach を使用している場合は、ANF の継続的な可用性を有効にします

**Category: Availability**

**Impact: Medium**

**Guidance**

Azure NetApp Files を使用している場合は、継続的可用性を有効にします。

各ファイル共有に接続しているユーザーの数を確認して、SMB パスがファイル接続の数を処理できることを確認します。現在、Azure Files では、ルート ディレクトリごとに最大 10k のハンドルがサポートされています。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/app-attach-overview?pivots=msix-app-attach)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-28/avd-28.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-29 - アプリのアタッチは別のファイル共有に配置する必要があり、ディザスター リカバリー プランにはアプリアタッチ ストレージを含める必要があります

**Category: Storage**

**Impact: Medium**

**Guidance**

アプリ添付パッケージは、プロファイルとは別の共有に配置する必要があります。また、アプリの添付ファイルはバックアップする必要があります。

ベスト プラクティスは、パフォーマンスとスケーラビリティの両方の目的で、アプリ アタッチ VHD ファイルをユーザー プロファイルから離れた別のファイル共有に分離することです。要件は、イメージに格納されているパッケージ化されたアプリケーションの数によって大きく異なる場合があり、要件を理解するにはアプリケーションをテストする必要があります。

ファイル共有は、セッション ホストと同じ Azure リージョンに存在する必要があります。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/app-attach-overview?pivots=msix-app-attach)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-29/avd-29.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-30 - 仮想ネットワークに、すべてのリージョンに対してルート テーブル/ルート サーバーが構成されていることを確認します

**Category: Networking**

**Impact: Medium**

**Guidance**

高可用性を実現するには、オンプレミスのデータセンターへの接続で、使用されているリージョン間のバックアップ パスを考慮する必要があります。セカンダリ リージョンにセカンダリ ルート テーブルを配置することで、ルーティングの冗長性を確保します。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering#need-for-redundant-connectivity-solution)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-30/avd-30.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-31 - 運用環境と DR 用に個別の IP 空間と NSG を使用して仮想ネットワークを分離します

**Category: Networking**

**Impact: Medium**

**Guidance**

AVD ペルソナごとの NSG と ASG、および Prod/DR リージョンごとの IP スペース。

組織が Azure での IP アドレス指定を計画することが重要です。計画により、IP アドレス空間がオンプレミスの場所と Azure リージョン間で重複しないようにします。オンプレミスと Azure リージョン間で IP アドレス空間が重複すると、競合に関する大きな課題が生じます。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-ip-addressing)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-31/avd-31.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-33 - ルートテーブルがフェイルオーバーに対応していることを確認します

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

FW/NVA へのトンネル トラフィックを強制するルート テーブルでフェールオーバーに関する考慮事項が評価され、失敗したり、次世代の FW 保護がトリガーされたりしないようにします。

AVD ワークロード チームは、ネットワークなどの共有インフラストラクチャを管理する一元化されたチームと協力して、ルーティングのフェールオーバーが期待どおりに実行されるように、運用ワークロードと DR ワークロードの両方に適切なルート テーブルがあることを確認する必要があります。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/landing-zone/design-area/management-business-continuity-disaster-recovery)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-33/avd-33.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-34 - ディザスター リカバリーのためセカンダリ Key Vault をプロビジョニングします

**Category: Disaster Recovery**

**Impact: High**

**Guidance:**
継続的な可用性とディザスター リカバリーの準備を確実にするために、セカンダリ リージョンにセカンダリ Key Vault をプロビジョニングすることをお勧めします。プライマリ リージョンで障害が発生した場合、このセカンダリ Key Vault により、セカンダリ リージョンでのデプロイで使用するために重要なシークレットにアクセスできるようになります。

**Resources:**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/key-vault/general/disaster-recovery-guidance)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-34/avd-34.kql" >}} {{< /code >}}

{{< /collapse >}}

### AVD-35 - AVD Insights Workbook を構成します

**Category: Monitoring**

**Impact: High**

**Guidance**

AVD Insights は、AVD 製品チームが提供する Azure Workbook テンプレートです。これは、メトリクス、ログ、イベントなどにわたる AVD ワークロードのモニタリングとトラブルシューティングを行うために強くお勧めします。運用環境と DR の両方のワークロードは、AVD Insights で有効にする必要があります。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/insights?tabs=monitor)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-35/avd-35.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-36 - 運用環境と DR 用に個別のログ分析ワークスペースを確保します

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

個別の Log Analytics を使用すると、DR 環境が完全に機能し、ワークロード チームがインシデントが発生した場合に依存するメトリック、パフォーマンス、その他の監査ツールを可視化できます。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/virtual-desktop/diagnostics-log-analytics)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-36/avd-36.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-37 - AVD ランディング ゾーンの方法論で記述されている AVD スケール ユニット モデルを使用して AVD リソースを整理します

**Category: Governance**

**Impact: Low**

**Guidance**

AVD ワークロードのリソースタイプと関連する共有リソースに基づいて複数のリソースグループを使用して、AVD ランディングゾーンのベストプラクティスに従います。

**Resources**

- [Learn More](https://learn.microsoft.com/ja-jp/azure/cloud-adoption-framework/scenarios/azure-virtual-desktop/enterprise-scale-landing-zone)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-37/avd-37.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
