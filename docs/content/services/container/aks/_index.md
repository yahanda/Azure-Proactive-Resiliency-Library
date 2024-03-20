+++
title = "AKS"
description = "Best practices and resiliency recommendations for AKS and associated resources."
date = "7/16/23"
author = "dcint"
msAuthor = "dacintro"
draft = false
+++

The presented resiliency recommendations in this guidance include Aks and associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                              |     Category      | Impact  |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:-------:|:-------:|:-------------------:|
| [AKS-1 - Deploy AKS cluster across availability zones](#aks-1---deploy-aks-cluster-across-availability-zones)                                                                                                               |   Availability    |  High   | Preview |         Yes         |
| [AKS-2 - Isolate system and application pods](#aks-2---isolate-system-and-application-pods)                                                                                                                                 |    Governance     |  High   | Preview |         Yes         |
| [AKS-3 - Disable local accounts](#aks-3---disable-local-accounts)                                                                                                                         | Access & Security |  High   | Preview |         Yes         |
| [AKS-4 - Configure Azure CNI networking for dynamic allocation of IPs](#aks-4---configure-azure-cni-networking-for-dynamic-allocation-of-ips)                                                                               |    Networking    | Medium  | Preview |         Yes         |
| [AKS-5 - Enable the cluster auto-scaler on an existing cluster](#aks-5---enable-the-cluster-auto-scaler-on-an-existing-cluster)                                                                                             | System Efficiency |  High   | Preview |         Yes         |
| [AKS-6 - Back up Azure Kubernetes Service](#aks-6---back-up-azure-kubernetes-service)                                                                                                                                       | Disaster Recovery |   Low   | Preview |         No          |
| [AKS-7 - Plan an AKS version upgrade](#aks-7---plan-an-aks-version-upgrade)                                                                                                                                                 |    Compliance     |  High   | Preview |         No          |
| [AKS-8 - Ensure that Persistent Volumes in storage account are redundant for Pods with stateful applications](#aks-8---ensure-that-persistent-volumes-in-storage-account-are-redundant-for-pods-with-stateful-applications) |   Availability    |   Low   | Preview |         No          |
| [AKS-9 - Upgrade Persistent Volumes with deprecated version to Azure CSI drivers](#aks-9---upgrade-persistent-volumes-with-deprecated-version-to-azure-csi-drivers)                                                         |   Storage  | High   | Preview |   No    |
| [AKS-10 - Implement Resource Quota to ensure that Kubernetes resources do not exceed hard resource limits.](#aks-10---implement-resource-quota-to-ensure-that-kubernetes-resources-do-not-exceed-hard-resource-limits)      | System Efficiency |   Low   | Preview |         No          |
| [AKS-11 - Attach Virtual Nodes (ACI) to the AKS cluster](#aks-11---attach-virtual-nodes-aci-to-the-aks-cluster)                                                                                                             | System Efficiency |   Low   | Preview |         No          |
| [AKS-12 - Update AKS tier to Standard](#aks-12---update-aks-tier-to-standard)                                                                                                                                               |   Availability   |  High   | Preview |         Yes         |
| [AKS-13 - Enable AKS Monitoring](#aks-13---enable-aks-monitoring)                                                                                                                                                           |    Monitoring     |  High   | Preview |         Yes         |
| [AKS-14 - Use Ephemeral Disks on AKS clusters](#aks-14---use-ephemeral-disks-on-aks-clusters)                                                                                                                               | System Efficiency | Medium  | Preview |         No          |
| [AKS-15 - Enable and remediate Azure Policies configured for AKS](#aks-15---enable-and-remediate-azure-policies-configured-for-aks)                                                                                         |    Governance     |   Low   | Preview |         No          |
| [AKS-16 - Enable GitOps when using DevOps frameworks](#aks-16---enable-gitops-when-using-devops-frameworks)                                                                                                                 |    Automation     |   Low   | Preview |         Yes         |
| [AKS-17 - Configure affinity or anti-affinity rules based on application requirements](#aks-17---configure-affinity-or-anti-affinity-rules-based-on-application-requirements)                                               |   Availability    |  High   | Preview |         No          |
| [AKS-18 - Configures Pods Liveness, Readiness, and Startup Probes](#aks-18---configures-pods-liveness-readiness-and-startup-probes)                                                                                         |   Availability    |  High   | Preview |         No          |
| [AKS-19 - Configure Pod replica sets in production applications to guarantee availability](#aks-19---configure-pod-replica-sets-in-production-applications-to-guarantee-availability)                                         |   Availability    |  High   | Preview |         No          |
| [AKS-20 - Configure system nodepool count](#aks-20---configure-system-nodepool-count)                                         |   Availability    |  High   | Preview |         Yes          |
| [AKS-21 - Configure user nodepool count](#aks-21---configure-user-nodepool-count)                                         |   Availability    |  High   | Preview |         Yes          |
| [AKS-22 - Configure pod disruption budgets (PDBs)](#aks-22---configure-pod-disruption-budgets-pdbs)                                         |   Availability    |  Medium   | Preview |         No          |
| [AKS-23 - Nodepool subnet size needs to accommodate maximum auto-scale settings](#aks-23---nodepool-subnet-size-needs-to-accommodate-maximum-auto-scale-settings)                                         |   Availability    |  High   | Preview |         No          |
| [AKS-24 - Enforce resource quotas at the namespace level](#aks-24---enforce-resource-quotas-at-the-namespace-level)                                         |   Availability    |  High   | Preview |         No          |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### AKS-1 - 可用性ゾーン間で AKS クラスターをデプロイします

**Category: Availability**

**Impact: High**

**Guidance**

Azure Availability Zones は、データセンター レベルの障害からアプリケーションとデータを保護する高可用性オファリングです。Availability Zones は、独立した電源、冷却装置、ネットワークを備えた Azure リージョン内の一意の物理的な場所です。各アベイラビリティーゾーンは 1 つ以上のデータセンターで構成され、高可用性とフォールトトレラントを実現するように設計されています。

aks クラスター、仮想マシン、ストレージ、データベースなどのリソースを同じリージョン内の複数の Availability Zones にデプロイすることで、データセンター レベルの障害からアプリケーションとデータを保護できます。1 つのアベイラビリティーゾーンがダウンしても、リージョン内の他のアベイラビリティーゾーンは引き続きサービスを提供できます。

**Resources**

- [AKS Availability Zones](https://learn.microsoft.com/ja-jp/azure/aks/availability-zones)
- [Zone Balancing](https://learn.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones#zone-balancing)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-1/aks-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-2 - システム ポッドとアプリケーション ポッドを分離します

**Category: Governance**

**Impact: High**

**Guidance**

AKS では、システム ノード プール内のノードに kubernetes.azure.com/mode: system というラベルが自動的に割り当てられます。このラベルは、このプール内のノードでシステム ポッドをスケジュールする必要があることを AKS に通知します。ただし、選択した場合は、これらのノードでアプリケーション Pod をスケジュールできます。

誤って構成されたアプリケーション Pod や不正なアプリケーション Pod が誤ってシステム Pod を強制終了するのを防ぐために、重要なシステム Pod をアプリケーション Pod から分離することをお勧めします。これは、専用ノード プールでシステム ポッドをスケジュールするか、ノード セレクターを使用して、システム ポッドが kubernetes.azure.com/mode: system ラベルを持つノードでのみスケジュールされるようにすることで実現できます。

**Resources**

- [System and user node pools](https://learn.microsoft.com/ja-jp/azure/aks/use-system-pools?tabs=azure-cli#system-and-user-node-pools)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-2/aks-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-3 - ローカル アカウントを無効化します

**Category: Access & Security**

**Impact: High**

**Guidance**

ローカル Kubernetes アカウントは、AKS クラスターにアクセスするための従来の監査不可能な手段であり、使用は推奨されません。AKS クラスターで Microsoft Entra 統合を有効にすると、クラスターへのアクセスを管理する上でいくつかの利点があります。Microsoft Entra を使用すると、ユーザーとグループの管理を一元化し、多要素認証を適用し、ロールベースのアクセス制御 (RBAC) を有効にして、クラスター リソースへのきめ細かなアクセス制御を行うことができます。さらに、Microsoft Entra は、他の Azure サービスやサードパーティの ID プロバイダーと統合できる安全でスケーラブルな認証メカニズムを提供します。

**Resources**

- [Entra integration](https://learn.microsoft.com/ja-jp/azure/aks/concepts-identity#azure-ad-integration)
- [Use Azure role-based access control for AKS](https://learn.microsoft.com/ja-jp/azure/aks/manage-azure-rbac?source=recommendations)
- [Manage AKS local accounts](https://learn.microsoft.com/ja-jp/azure/aks/manage-local-accounts-managed-azure-ad?source=recommendations)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-3/aks-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-4 - IP の動的割り当て用に Azure CNI ネットワークを構成します

**Category: Networking**

**Impact: Medium**

**Guidance**

Azure CNI ネットワーク ソリューションには、ポッドへの IP の動的割り当て、ノードとポッドのサブネットの個別にスケーリング、VNET 内のポッドとリソース間の直接ネットワーク接続、ポッドとノードに異なるネットワーク ポリシーを許可するなど、クラスター ポッドの IP アドレスとネットワーク接続を管理するためのいくつかの利点があります。また、Azure ネットワーク ポリシーや Calico など、さまざまなネットワーク ポリシーもサポートしています。

**Resources**

- [Configure Azure CNI networking](https://learn.microsoft.com/ja-jp/azure/aks/configure-azure-cni-dynamic-ip-allocation)
- [Configure Azure CNI Overlay networking](https://learn.microsoft.com/ja-jp/azure/aks/azure-cni-overlay)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-4/aks-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-5 - 既存のクラスターでクラスター オートスケーラーを有効にします

**Category: System Efficiency**

**Impact: High**

**Guidance**

クラスター オートスケーラーは、ポッド リソース要求とクラスター内の使用可能な容量に基づいて、ノード プール内のノード数を自動的にスケーリングします。これにより、クラスターを需要に応じてスケーリングし、停止を防ぐことができます。

クラスターでアベイラビリティーゾーンが有効になっている場合は、次の構成変更を検証または確立する必要があります。

- 永続ボリューム - クラスターで Azure Storage によってサポートされる永続ボリュームを使用している場合は、可用性ゾーンごとに 1 つのノードプールがあることを確認します。永続ボリュームは AZ 間では機能せず、ノードプールが永続ボリュームにアクセスできない場合、オートスケーラーは新しいポッドの作成に失敗する可能性があります。
- ゾーンごとに複数のノードプール - クラスターに AZ ごとに複数のノードプールがある場合は、自動スケーラー プロファイルを使用して '--balance-similar-node-groups' プロパティを有効にします。この機能は、類似したノードプールを検出し、それらの間でノードの数のバランスを取ります。


**Resources**

- [Use the Cluster Autoscaler on AKS](https://learn.microsoft.com/ja-jp/azure/aks/cluster-autoscaler?tabs=azure-cli)
- [Best practices for advanced scheduler features](https://learn.microsoft.com/ja-jp/azure/aks/operator-best-practices-advanced-scheduler)
- [Node pool scaling considerations and best practices](https://learn.microsoft.com/ja-jp/azure/aks/best-practices-performance-scale-large#node-pool-scaling)
- [Best practices for basic scheduler features](https://learn.microsoft.com/ja-jp/azure/aks/operator-best-practices-scheduler)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-5/aks-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-6 - Azure Kubernetes Service をバックアップします

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

AKS は、バックアップ戦略を必要とするステートフル アプリケーションに使用されることが増えています。Azure Backup では、クラスターにインストールする必要があるバックアップ拡張機能を使用して、AKS クラスター (クラスターに接続されているクラスター リソースと永続ボリューム) をバックアップできるようになりました。バックアップ コンテナーは、このバックアップ拡張機能を介してクラスターと通信し、バックアップと復元の操作を実行します。

**Resources**

- [AKS Backups](https://learn.microsoft.com/ja-jp/azure/backup/azure-kubernetes-service-cluster-backup)
- [Best Practices for AKS Backups](https://learn.microsoft.com/ja-jp/azure/aks/operator-best-practices-storage)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-6/aks-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-7 - AKS バージョンのアップグレードを計画します

**Category: Compliance**

**Impact: High**

**Guidance**

マイナーバージョンのリリースには、新機能と改善が含まれています。パッチリリースはより頻繁(場合によっては毎週)で、マイナーバージョン内の重大なバグ修正を目的としています。パッチリリースには、セキュリティの脆弱性や重大なバグの修正が含まれています。
サポートされていない Kubernetes バージョンを実行している場合は、クラスターのサポートを要求するときにアップグレードするように求められます。サポートされていない Kubernetes リリースを実行しているクラスターは、AKS サポート ポリシーの対象外です。

**Resources**

- [Updating to the latest AKS version](https://learn.microsoft.com/ja-jp/azure/aks/operator-best-practices-cluster-security?tabs=azure-cli#regularly-update-to-the-latest-version-of-kubernetes)
- [Upgrade cluster](https://learn.microsoft.com/ja-jp/azure/aks/upgrade-cluster?tabs=azure-cli)
- [Auto-upgrading cluster](https://learn.microsoft.com/ja-jp/azure/aks/auto-upgrade-cluster)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-7/aks-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-8 - ストレージ アカウント内の永続ボリュームがステートフル アプリケーションを含むポッドに対して冗長であることを確認します

**Category: Availability**

**Impact: Low**

**Guidance**

Azure Storage アカウント内のデータは、常にプライマリ リージョンで 3 回レプリケートされます。Azure Storage for Persistent Volumes には、プライマリ リージョンまたはペア リージョンでデータをレプリケートする方法に関する他のオプションが用意されています。

- LRS は、1 つの物理的な場所でデータを 3 回同期的にレプリケートします。レプリケーションは最もコストがかかりませんが、高可用性と持続性を備えたアプリにはお勧めしません。LRS は イレブン ナイン の耐久性を提供します。
- ZRS は、プライマリ リージョンの 3 つの可用性ゾーン間でデータを同期的にコピーします。ZRS は、ゾーン間で高可用性を必要とするアプリに推奨されます。ZRS は トゥエルブ ナイン の耐久性を提供します。

AKS では、Premium_ZRS と StandardSSD_ZRS のディスクの種類がサポートされています。ZRS ディスクは、特定のノードと同じゾーンにディスク ボリュームを併置する必要があるという制限なしに、ゾーン ノードまたは非ゾーン ノードでスケジュールできます。

**Resources**

- [Azure Disk CSI Driver](https://learn.microsoft.com/ja-jp/azure/aks/azure-disk-csi#azure-disk-csi-driver-features)
- [Virtual Machine Disk Redundancy](https://learn.microsoft.com/ja-jp/azure/virtual-machines/disks-redundancy)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-8/aks-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-9 - 非推奨バージョンの永続ボリュームを Azure CSI ドライバーにアップグレードします

**Category: Storage**

**Impact: High**

**Guidance**

Kubernetes バージョン 1.26 以降、ツリー内の永続ボリュームの種類 kubernetes.io/azure-disk と kubernetes.io/azure-file は非推奨となり、サポートされなくなります。廃止後にこれらのドライバーを削除する予定はありませんが、対応する CSI ドライバーに disks.csi.azure.com および file.csi.azure.com 移行する必要があります。

**Resources**

- [CSI Storage Drivers](https://learn.microsoft.com/ja-jp/azure/aks/csi-storage-drivers)
- [CSI Migrate in Tree Volumes](https://learn.microsoft.com/ja-jp/azure/aks/csi-migrate-in-tree-volumes)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-9/aks-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-10 - Kubernetes リソースがハード リソース制限を超えないようにリソース クォータを実装します

**Category: System Efficiency**

**Impact: Low**

**Guidance**

リソース クォータは、ResourceQuota オブジェクトによって定義され、名前空間ごとのリソース消費量の総量を制限する制約を提供します。これにより、名前空間で作成できるオブジェクトの数を種類別に制限したり、その名前空間のリソースによって消費される可能性のあるコンピューティング リソースの合計量を制限したりできます。

**Resources**

- [Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-10/aks-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-11 - AKS クラスターに仮想ノード (ACI) をアタッチします

**Category: System Efficiency**

**Impact: Low**

**Guidance**

AKS クラスター内のアプリケーション ワークロードを迅速にスケーリングするには、仮想ノードを使用できます。仮想ノードを使用すると、ポッドは Kubernetes クラスター オートスケーラーを使用するよりもはるかに高速にプロビジョニングされます。

クラスターでアベイラビリティーゾーンが有効になっている場合は、次の構成変更を検証または確立する必要があります。

- 永続ボリューム - クラスターで Azure Storage によってサポートされる永続ボリュームを使用している場合は、可用性ゾーンごとに 1 つのノードプールがあることを確認します。永続ボリュームは AZ 間では機能せず、ノードプールが永続ボリュームにアクセスできない場合、オートスケーラーは新しいポッドの作成に失敗する可能性があります。

**Resources**

- [Virtual Nodes](https://learn.microsoft.com/ja-jp/azure/aks/virtual-nodes)
- [Azure Container Instances](https://learn.microsoft.com/ja-jp/azure/container-instances/container-instances-overview)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-11/aks-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-12 - AKS レベルを Standard に更新します

**Category: Availability**

**Impact: High**

**Guidance**

運用 AKS クラスターは、Standard レベルで構成する必要があります。AKS 無料サービスでは、返金制度のある SLA は提供されず、ノードのスケーラビリティは制限されます。その SLA を取得するには、Standard レベルを選択する必要があります。

**Resources**

- [Pricing Tiers](https://learn.microsoft.com/ja-jp/azure/aks/free-standard-pricing-tiers)
- [AKS Baseline Architecture](https://learn.microsoft.com/ja-jp/azure/architecture/reference-architectures/containers/aks/baseline-aks?toc=https%3A%2F%2Flearn.microsoft.com%2Fja-jp%2Fazure%2Faks%2Ftoc.json&bc=https%3A%2F%2Flearn.microsoft.com%2Fja-jp%2Fazure%2Fbread%2Ftoc.json#kubernetes-api-server-sla)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-12/aks-12.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-13 - AKS 監視を有効にします

**Category: Monitoring**

**Impact: High**

**Guidance**

Azure Monitor は、イベントを収集し、コンテナー ログをキャプチャし、メトリック API から CPU/メモリ情報を収集し、データの視覚化を可能にして、AKS 環境のほぼリアルタイムの正常性とパフォーマンスを検証します。視覚化ツールには、Azure Monitor Container Insights、Prometheus、Grafana などがあります。

**Resources**

- [Monitor AKS](https://learn.microsoft.com/ja-jp/azure/aks/monitor-aks)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-13/aks-13.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-14 - AKS クラスターでエフェメラル ディスクを使用します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

エフェメラル OS ディスクはローカルに接続され、マネージド ディスクとしてレプリケートされないため、AKS エージェント ノードの OS ディスクでの読み取り/書き込み待機時間が短くなります。また、再イメージ化と起動時間の短縮により、スケーリングやアップグレードなどのクラスター操作も高速化されます。

**Resources**

- [AKS Ephemeral OS Disk](https://learn.microsoft.com/samples/azure-samples/aks-ephemeral-os-disk/aks-ephemeral-os-disk/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-14/aks-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-15 - AKS 用に構成された Azure ポリシーを有効にして修復します

**Category: Governance**

**Impact: Low**

**Guidance**

Azure ポリシーを使用すると、企業は、セキュリティ、認証、プロビジョニング、ネットワークなどに関するガバナンスのベスト プラクティスを AKS クラスターに適用できます。

**Resources**

- [AKS Baseline - Policy Management](https://learn.microsoft.com/ja-jp/azure/architecture/reference-architectures/containers/aks/baseline-aks?toc=https%3A%2F%2Flearn.microsoft.com%2Fja-jp%2Fazure%2Faks%2Ftoc.json&bc=https%3A%2F%2Flearn.microsoft.com%2Fja-jp%2Fazure%2Fbread%2Ftoc.json#policy-management)
- [Built-in Policy Definitions for AKS](https://learn.microsoft.com/ja-jp/azure/aks/policy-reference)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-15/aks-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-16 - DevOps フレームワークの使用時に GitOps を有効にします

**Category: Automation**

**Impact: Low**

**Guidance**

GitOps は、アプリケーションと宣言型インフラストラクチャ コードを Git に格納し、自動化された継続的デリバリーの信頼できる情報源として使用するクラウドネイティブ アプリケーションの運用モデルです。GitOps では、システム全体の望ましい状態を git リポジトリに記述し、GitOps オペレーターがそれを環境 (多くの場合は Kubernetes クラスター) にデプロイします。潜在的な停止やフェールオーバーの失敗のシナリオを防ぐために、GitOps は、すべての AKS クラスターの構成を意図した構成に維持するのに役立ちます。

**Resources**

- [GitOps with AKS](https://learn.microsoft.com/ja-jp/azure/architecture/guide/aks/aks-cicd-github-actions-and-gitops)
- [GitOps for AKS - Reference Architecture](https://learn.microsoft.com/ja-jp/azure/architecture/example-scenario/gitops-aks/gitops-blueprint-aks)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-16/aks-16.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-17 - アプリケーション要件に基づいてアフィニティまたは非アフィニティのルールを構成します

**Category: Availability**

**Impact: High**

**Guidance**

Topology Spread Constraintsを設定して、リージョン、ゾーン、ノード、その他のユーザー定義のトポロジードメインなどの障害ドメイン間でPodをクラスター全体に分散する方法を制御します。これにより、高可用性と効率的なリソース使用率を実現できます。

**Resources**

- [Topology Spread Constraints](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/)
- [Assign Pod Node](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-17/aks-17.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-18 - ポッドの Liveness、Readiness、Startup プローブを構成します

**Category: Availability**

**Impact: High**

**Guidance**

AKS kubelet コントローラーでは、Liveness Probe を使用してコンテナーとアプリケーションの正常性を検証します。コンテナの健全性に基づいて、kubeletはコンテナを再起動するタイミングを認識します。

**Resources**

- [Configure probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
- [Assign Pod Node](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-18/aks-18.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-19 - 可用性を保証するために運用アプリケーションでポッド レプリカ セットを構成します

**Category: Availability**

**Impact: High**

**Guidance**

PodまたはDeploymentマニフェストでReplicaSetを設定して、安定したレプリカPodのセットをいつでも実行し続けるようにします。この機能は、指定された数の同一のPodの可用性を保証します。

**Resources**

- [Replica Sets](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-19/aks-19.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-20 - システム ノードプール数を構成します

**Category: Availability**

**Impact: High**

**Guidance**

システム ノード プールは、重要なシステム ポッドがノードの停止に対して回復性があることを確認するために、最小ノード数を 2 に設定して構成する必要があります。

**Resources**

- [System nodepools](https://learn.microsoft.com/ja-jp/azure/aks/use-system-pools?tabs=azure-cli)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-20/aks-20.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-21 - ユーザー ノードプール数を構成します

**Category: Availability**

**Impact: High**

**Guidance**

アプリケーションで高可用性が必要な場合は、ユーザー ノード プールを 2 以上のノード数で構成する必要があります。

**Resources**

- [Azure Well-Architected Framework review for Azure Kubernetes Service (AKS)](https://learn.microsoft.com/ja-jp/azure/well-architected/service-guides/azure-kubernetes-service#design-checklist)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-21/aks-21.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-22 - ポッド中断バジェット (PDB) を構成します

**Category: Availability**

**Impact: Medium**

**Guidance**

Pod Disruption Budget (PDB) は、メンテナンスやスケーリング イベントなどの自発的な中断時に使用可能なポッドの最小数または割合を構成できる Kubernetes リソースです。アプリケーションの可用性を維持するには、Pod Disruption Budgets (PDB)を定義して、クラスタで使用可能なポッドの最小数を確保します。

**Resources**

- [Configure PDBs](https://kubernetes.io/docs/tasks/run-application/configure-pdb/)
- [Plan availability using PDBs](https://learn.microsoft.com/ja-jp/azure/aks/operator-best-practices-scheduler#plan-for-availability-using-pod-disruption-budgets)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-22/aks-22.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-23 - ノードプールのサブネット サイズは、最大自動スケール設定に対応する必要があります

**Category: Availability**

**Impact: High**

**Guidance**

ノードプール サブネットのサイズは、最大自動スケール設定に対応する必要があります。サブネットのサイズを適切に設定することで、AKS はノードを効率的にスケールアウトして需要の増加に対応し、リソースの制約や潜在的なサービス中断のリスクを軽減できます。

**Resources**

- [AKS Networking](https://learn.microsoft.com/ja-jp/azure/aks/concepts-network)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-23/aks-23.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AKS-24 - 名前空間レベルでリソース クォータを適用します

**Category: Availability**

**Impact: High**

**Guidance**

名前空間レベルのリソース クォータを適用することは、リソースの枯渇を防ぎ、クラスターの安定性を維持することで信頼性を確保するために重要です。これにより、個々のアプリケーションやユーザーがリソースを独占し、パフォーマンスの低下やクラスタ内の他のアプリケーションの停止につながるのを防ぐことができます。

**Resources**

- [Resource quotas](https://learn.microsoft.com/ja-jp/azure/aks/operator-best-practices-scheduler#enforce-resource-quotas)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aks-24/aks-24.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
