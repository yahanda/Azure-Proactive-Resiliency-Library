+++
title = "Container Registry"
description = "Best practices and resiliency recommendations for Container Registries and associated resources."
date = "8/21/23"
author = "oZakari"
msAuthor = "ztrocinski"
draft = false
+++

The presented resiliency recommendations in this guidance include Container Registries and dependent resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [CR-1 - Use Premium tier for critical production workloads](#cr-1---use-premium-tier-for-critical-production-workloads) | System Efficiency | High | Preview | Yes |
| [CR-2 - Enable zone redundancy](#cr-2---enable-zone-redundancy) | Availability | High | Preview | Yes |
| [CR-3 - Enable geo-replication](#cr-3---enable-geo-replication) | Disaster Recovery | High | Preview | Yes |
| [CR-5 - Use Repository namespaces](#cr-5---use-repository-namespaces) | Access & Security | Low | Preview | No |
| [CR-6 - Move Container Registry to a dedicated resource group](#cr-6---move-container-registry-to-a-dedicated-resource-group) | Governance | Low | Preview | Yes |
| [CR-7 - Manage registry size](#cr-7---manage-registry-size) | System Efficiency | Medium | Preview | No |
| [CR-8 - Disable anonymous pull access](#cr-8---disable-anonymous-pull-access) | Access & Security | Medium | Preview | Yes |
| [CR-10 - Configure Diagnostic Settings for all Azure Container Registries](#cr-10---configure-diagnostic-settings-for-all-azure-container-registries) | Monitoring | Medium | Preview | No |
| [CR-11 - Monitor Azure Container Registry with Azure Monitor](#cr-11---monitor-azure-container-registry-with-azure-monitor) | Monitoring | Medium | Preview | No |
| [CR-12 - Enable soft delete policy](#cr-12---enable-soft-delete-policy) | Disaster Recovery | Medium | Preview | Yes |
{{< /table >}}

{{< alert style="info" >}}
Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})
{{< /alert >}}

## Recommendations Details

### CR-1 - 重要な運用ワークロードに Premium レベルを使用します

**Category: System Efficiency**

**Impact: High**

**Guidance**

パフォーマンスのニーズを満たす Azure Container Registry のサービス レベルを選択します。Premium レベルでは、大規模なデプロイ時に最大の帯域幅と同時読み取りおよび書き込み操作のレートが最大になります。作業の開始には Basic、ほとんどの運用アプリケーションには Standard、ハイパースケール パフォーマンスと geo レプリケーションには Premium を使用します。

**Resources**

- [Container Registry Best Practices](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-best-practices)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-1/cr-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CR-2 - ゾーン冗長性を有効にします

**Category: Availability**

**Impact: High**

**Guidance**

Azure Container Registry では、オプションのゾーン冗長性がサポートされています。ゾーン冗長性は、特定のリージョン内のレジストリまたはレプリケーション リソース (レプリカ) に回復性と高可用性を提供します。

**Resources**

- [Registry best practices - Enable zone redundancy](https://review.learn.microsoft.com/ja-jp/azure/container-registry/zone-redundancy?toc=%2Fazure%2Freliability%2Ftoc.json&bc=%2Fazure%2Freliability%2Fbreadcrumb%2Ftoc.json&branch=main)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-2/cr-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CR-3 - geo レプリケーションを有効にします

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

コンテナーを複数のリージョンにデプロイする場合は、Azure Container Registry の geo レプリケーション機能を使用します。ローカル データ センターから世界中の顧客にサービスを提供している場合でも、開発チームがさまざまな場所にいる場合でも、レジストリを geo レプリケートすることで、レジストリ管理を簡素化し、待機時間を最小限に抑えることができます。また、リージョン Webhook を構成して、イメージがプッシュされたときなど、特定のレプリカのイベントを通知することもできます。

geo レプリケーションは、Premium レジストリで使用できます。

**Resources**

- [Registry best practices - Enable geo-replication](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-best-practices#geo-replicate-multi-region-deployments)
- [Geo-Replicate Container Registry](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-geo-replication)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-3/cr-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CR-5 - リポジトリ名前空間を使用します

**Category: Access & Security**

**Impact: Low**

**Guidance**

リポジトリ名前空間を使用すると、組織内の複数のグループ間で 1 つのレジストリを共有できます。レジストリは、デプロイ間およびチーム間で共有できます。Azure Container Registry では、入れ子になった名前空間がサポートされており、グループの分離が可能です。ただし、レジストリは、すべてのリポジトリを階層としてではなく、個別に管理します。

**Resources**

- [Registry best practices - use repository namespaces](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-best-practices#repository-namespaces)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-5/cr-5.kql" >}} {{< /code >}}

{{< /collapse >}}
<br><br>

### CR-6 - コンテナー レジストリを専用リソース グループに移動します

**Category: Governance**

**Impact: Low**

**Guidance**

コンテナー レジストリは複数のコンテナー ホストで使用されるリソースであるため、レジストリは独自のリソース グループに存在する必要があります。

Azure Container Instances などの特定のホストの種類を試すこともできますが、完了したらコンテナー インスタンスを削除することをお勧めします。ただし、Azure Container Registry にプッシュしたイメージのコレクションを保持することもできます。レジストリを独自のリソース グループに配置することで、コンテナー インスタンスのリソース グループを削除するときに、レジストリ内のイメージのコレクションを誤って削除するリスクを最小限に抑えることができます。

**Resources**

- [Registry best practices - Use dedicated resource group](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-best-practices#dedicated-resource-group)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-6/cr-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CR-7 - レジストリ サイズを管理します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

各コンテナー レジストリ サービス レベルのストレージの制約は、一般的なシナリオ (作業を開始する場合は Basic、ほとんどの運用アプリケーションの場合は Standard、ハイパースケール パフォーマンスと geo レプリケーションの場合は Premium) に合わせて調整することを目的としています。レジストリの有効期間中は、未使用のコンテンツを定期的に削除してサイズを管理する必要があります。また、アイテム保持ポリシーを有効にして、タグ付けされていないイメージ マニフェストを自動的に削除し、ストレージ領域を解放することも検討してください。

**Resources**

- [Registry best practices - Manage registry size](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-best-practices#manage-registry-size)
- [Retention Policy](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-retention-policy#about-the-retention-policy)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-7/cr-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CR-8 - 匿名プルアクセスを無効化します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

既定では、Azure コンテナー レジストリからコンテンツをプルまたはプッシュするためのアクセスは、認証されたユーザーのみが使用できます。匿名 (認証されていない) プル アクセスを有効にすると、すべてのレジストリ コンテンツを読み取り (プル) アクションで公開できるようになります。警告: 匿名プル アクセスは、現在、レジストリ内のすべてのリポジトリに適用されます。リポジトリ スコープのトークンを使用してリポジトリ アクセスを管理する場合、すべてのユーザーは、匿名プルが有効になっているレジストリ内のリポジトリからプルできます。匿名プル アクセスが有効になっている場合は、トークンを削除することをお勧めします。

**Resources**

- [Enable anonymous pull access](https://learn.microsoft.com/ja-jp/azure/container-registry/anonymous-pull-access#about-anonymous-pull-access)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-8/cr-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CR-10 - すべての Azure コンテナー レジストリの診断設定を構成します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

リソース ログは、診断設定を作成して 1 つ以上の場所にルーティングするまで収集および保存されません。

**Resources**

- [Monitoring Azure Container Registry data reference - Resource Logs](https://learn.microsoft.com/ja-jp/azure/container-registry/monitor-service-reference#resource-logs)
- [Monitor Azure Container Registry - Enable diagnostic logs](https://learn.microsoft.com/ja-jp/azure/container-registry/monitor-service#collection-and-routing)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-10/cr-10.kql" >}} {{< /code >}}

{{< /collapse >}}
<br><br>

### CR-11 - Azure Monitor を使用して Azure Container Registry を監視します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

Azure リソースに依存する重要なアプリケーションやビジネス プロセスがある場合は、それらのリソースの可用性、パフォーマンス、操作を監視する必要があります。Azure Container Registry は、他のクラウドやオンプレミスのリソースに加えて、Azure リソースを監視するための完全な機能セットを提供する Azure のフル スタック監視サービスである Azure Monitor を使用して監視データを作成します。

**Resources**

- [Monitoring Azure Container Registry data reference](https://learn.microsoft.com/ja-jp/azure/container-registry/monitor-service-reference#metrics)
- [Monitor Azure Container Registry](https://learn.microsoft.com/ja-jp/azure/container-registry/monitor-service)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-11/cr-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### CR-12 - 論理的な削除ポリシーを有効にします

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

論理的な削除ポリシーを有効にすると、ACR は、削除された成果物を、保持期間が設定された論理的に削除された成果物として管理します。これにより、論理的に削除された成果物を一覧表示、フィルター処理、および復元できます。保持期間が完了すると、論理的に削除されたすべての成果物が自動的に消去されます。

**Resources**

- [Enable soft delete policy](https://learn.microsoft.com/ja-jp/azure/container-registry/container-registry-soft-delete-policy)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/cr-12/cr-12.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
