+++
title = "2 - Design"
description = "Microsoft Azure Well-Architected Framework best practices and recommendations for the Reliability Stage - 2 - Design"
date = "9/18/23"
weight = 2
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented Microsoft Azure Well-Architected Framework recommendations in this guidance include Reliability Stage "2 - Design (Workload Design)" and associated resources and their settings.

In this Stage, the architecture and design decisions are made to meet the requirements defined earlier. Best practices for resilient and scalable systems are implemented, often including redundancy, failover strategies, and load balancing. In this phase failure mode and point analysis are coordinated in order to identify and mitigate possible failures, and single points of failures.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                          |  Category         |  Impact   |  State   | ARG Query Available |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------:  | :------:  | :------: | :-----------------: |
| [WADS-1 - Consider deploying your application across multiple zones](#wads-1---consider-deploying-your-application-across-multiple-zones)                                                               | Availability      |   High    | Verified |         No          |
| [WADS-2 - Consider deploying your application across multiple regions](#wads-2---consider-deploying-your-application-across-multiple-regions)                                                           | Disaster Recovery |   High    | Verified |         No          |
| [WADS-3 - Ensure that all fault-points and fault-modes are understood and operationalized](#wads-3---ensure-that-all-fault-points-and-fault-modes-are-understood-and-operationalized)                   | Availability      |   High    | Verified |         No          |
| [WADS-4 - Use PaaS Azure services instead of IaaS](#wads-4---use-paas-azure-services-instead-of-iaas)                                                                                                    | System Efficiency |   Medium  | Verified |         No          |
| [WADS-5 - Design the application to scale out](#wads-5---design-the-application-to-scale-out)                                                                                                           | System Efficiency |   High    | Verified |         No          |
| [WADS-6 - Create a landing zone for the workload following the Microsoft Cloud Adoption Framework](#wads-6---create-a-landing-zone-for-the-workload-following-the-microsoft-cloud-adoption-framework)   | Governance        |   Low     | Verified |         No          |
| [WADS-7 - Design a BCDR strategy that will help to meet the business requirements](#wads-7---design-a-bcdr-strategy-that-will-help-to-meet-the-business-requirements)                                   | Disaster Recovery |   High    | Verified |         No          |
| [WADS-8 - Provide security assurance through identity management](#wads-8---provide-security-assurance-through-identity-management)                                                                     | Access & Security |   Medium  | Verified |         No          |
| [WADS-9 - Ensure you address security-related risks helps to minimize application downtime and data loss caused by unexpected security exposures](#wads-9---ensure-you-address-security-related-risks-helps-to-minimize-application-downtime-and-data-loss-caused-by-unexpected-security-exposures) |  Access & Security |   High    | Verified |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### WADS-1 - アプリケーションを複数のゾーンにデプロイすることを検討します

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

リージョン内で可用性ゾーンを使用するようにアプリケーション アーキテクチャを設計します。可用性ゾーンを使用すると、データセンター レベルのフォールト トレランスを提供することで、リージョン内のアプリケーションの可用性を最適化できます。ただし、アプリケーションアーキテクチャは、ゾーンを効果的に使用するために、ゾーン間で依存関係を共有してはなりません。

アプリケーションのパフォーマンス上の理由から、コンポーネントの近接性が必要かどうかを検討します。アプリケーションの全部または一部が待機時間の影響を強く受ける場合は、コンポーネントを同じ場所に配置する必要があるため、マルチリージョンおよびマルチゾーン戦略の適用が制限される可能性があります。

**Resources**

- [Use Availability Zones](https://learn.microsoft.com/ja-jp/azure/reliability/availability-zones-overview#availability-zones)

<br><br>

### WADS-2 - 複数のリージョンにアプリケーションをデプロイすることを検討します

**Category: Disaster Recovery**

**Impact: High**

**Recommendation/Guidance**

アプリケーションが 1 つのリージョンにデプロイされ、そのリージョンが使用できなくなった場合、アプリケーションも使用できなくなります。これは、アプリケーションの SLA の条件の下では受け入れられない可能性があります。

その場合は、アプリケーションとそのサービスを複数のリージョンにデプロイすることを検討してください。マルチリージョン展開では、アクティブ/アクティブまたはアクティブ/パッシブ構成を使用できます。

アクティブ/アクティブ構成では、複数のアクティブ領域に要求が分散されます。アクティブ/パッシブ構成では、ウォーム インスタンスはセカンダリ リージョンに保持されますが、プライマリ リージョンに障害が発生しない限り、そこにトラフィックは送信されません。

**Resources**

- [Design reliable Azure applications](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/app-design)
- [Cross-region replication in Azure: Business continuity and disaster recovery](https://learn.microsoft.com/ja-jp/azure/reliability/cross-region-replication-azure)

<br><br>

### WADS-3 - すべての障害ポイントと障害モードを認識して運用できるようにすることを確認します

**Category: Availability**

**Impact: High**

**Recommendation/Guidance**

すべての障害ポイントと障害モードが理解され、運用可能であることを確認します。

故障モード分析 (FMA) は、システム内の潜在的な障害ポイントを特定することにより、システムに回復性を構築するためのプロセスです。FMA は、障害回復を最初からシステムに組み込めるように、アーキテクチャと設計のフェーズの一部である必要があります。

すべての障害点と障害モードを特定します。障害点は、アプリケーションアーキテクチャ内で障害が発生する可能性のある要素を記述し、障害モードは、障害点が障害を起こす可能性のあるさまざまな方法をキャプチャします。アプリケーションがエンドツーエンドの障害に対して回復力があることを確認するには、すべての障害点と障害モードを理解し、運用可能にすることが不可欠です。

**Resources**

- [Failure mode analysis for Azure applications](https://learn.microsoft.com/ja-jp/azure/architecture/resiliency/failure-mode-analysis)

<br><br>

### WADS-4 - IaaS の代わりに PaaS Azure サービスを使用します

**Category: System Efficiency**

**Impact: Medium**

**Recommendation/Guidance**

PaaS は、アプリを開発および実行するためのフレームワークを提供します。IaaS と同様に、PaaS プロバイダーは、プラットフォームのサーバー、ネットワーク、ストレージ、その他のコンピューティング リソースをホストおよび維持します。ただし、PaaS には、Web アプリケーションのライフサイクルをサポートするツール、サービス、システムも含まれています。開発者は、このプラットフォームを使用して、バックアップ、セキュリティソリューション、アップグレード、その他の管理タスクを管理することなく、アプリを構築できます。

**Resources**

- [Use platform as a service (PaaS) options](https://learn.microsoft.com/ja-jp/azure/architecture/guide/design-principles/managed-services)

<br><br>

### WADS-5 - スケールアウトするアプリケーションを設計します

**Category: System Efficiency**

**Impact: High**

**Recommendation/Guidance**

Azure には柔軟なスケーラビリティが用意されているため、スケールアウトするように設計する必要があります。ただし、アプリケーションでは、スケール ユニット アプローチを利用してサービスとサブスクリプションの制限を回避し、個々のコンポーネントとアプリケーション全体を水平方向にスケーリングできるようにする必要があります。コストを削減するために重要なスケールインを忘れないでください。たとえば、App Service のスケールインとスケールアウトは、ルールを使用して行われます。多くの場合、お客様はスケールアウト ルールを記述し、スケールイン ルールを記述しないため、App Service のコストが高くなります。

**Resources**

- [Design to scale out](https://learn.microsoft.com/ja-jp/azure/architecture/guide/design-principles/scale-out)

<br><br>

### WADS-6 - Microsoft クラウド導入フレームワークに従ったワークロードのランディング ゾーンを作成します

**Category: Governance**

**Impact: Low**

**Recommendation/Guidance**

ワークロードの観点から見ると、ランディング ゾーンとは、アプリケーションがデプロイされる準備されたプラットフォームを指します。ランディング ゾーンの実装には、コンピューティング、データ ソース、アクセス制御、およびネットワーク コンポーネントが既にプロビジョニングされている場合があります。必要な配管の準備ができた状態で。ワークロードを接続する必要があります。

全体的なセキュリティを考慮すると、ランディング ゾーンは、ワークロードの脅威軽減レイヤーを追加する一元化されたセキュリティ機能を提供します。実装はさまざまですが、セキュリティ体制を強化する一般的な戦略をいくつか紹介します。

- セグメンテーションによる分離。資産は、Azure の登録から、ワークロードのリソースを持つサブスクリプションまで、複数のレイヤーで分離できます。
- 組織のポリシーを一貫して採用し、Azure Policy を使用してサービスとその構成の作成と削除を強制します。
- ゼロトラストの原則に沿った構成。たとえば、実装には、オンプレミスのデータ センターへのネットワーク接続がある場合があります。

**Resources**

- [Azure landing zone integration](https://learn.microsoft.com/ja-jp/azure/well-architected/security/design-governance-landing-zone)

<br><br>

### WADS-7 - ビジネス要件を満たすのに役立つ BCDR 戦略を設計します

**Category: Disaster Recovery**

**Impact: High**

**Recommendation/Guidance**

ディザスタリカバリは、壊滅的な損失の後にアプリケーションの機能を復元するプロセスです。クラウド環境では、障害が発生することを前もって認識しています。目標は、障害を完全に防ごうとするのではなく、1 つのコンポーネントに障害が発生した場合の影響を最小限に抑えることです。

テストは、これらの影響を最小限に抑える方法の 1 つです。アプリケーションのテストは可能な限り自動化する必要がありますが、失敗した場合に備える必要もあります。障害が発生した場合は、バックアップとリカバリの戦略が重要になります。障害発生時の機能低下に対する許容度は、アプリケーションごとに異なるビジネス上の決定事項です。

一部のアプリケーションが一時的に使用不可になったり、機能が制限されたり、処理が遅れたりして部分的に使用可能になることは許容できる場合があります。他のアプリケーションでは、機能の低下は許容されません。

キーポイント:

- 主要な障害シナリオを使用して、ディザスタリカバリ計画を定期的に作成し、テストします。
- ほとんどのアプリケーションを機能制限して実行するためのディザスタリカバリ戦略を設計します。
- アプリケーションのビジネス要件と状況に合わせたバックアップ戦略を設計します。
- フェイルオーバーとフェイルバックのステップとプロセスを自動化します。
- フェールオーバーとフェールバックのアプローチを少なくとも 1 回は正常にテストして検証します。

**Resources**

- [Backup and disaster recovery for Azure applications](https://learn.microsoft.com/ja-jp/azure/well-architected/resiliency/backup-and-recovery)

<br><br>

### WADS-8 - ID 管理によるセキュリティ保証を提供します

**Category: Access & Security**

**Impact: Medium**

**Recommendation/Guidance**

ID 管理 (セキュリティ プリンシパルを認証および承認するプロセス) を通じてセキュリティを保証します。ID 管理サービスを使用して、ユーザー、パートナー、お客様、アプリケーション、サービス、およびその他のエンティティを認証し、アクセス許可を付与します。ID 管理は、通常、ワークロードのアーキテクチャの一部としてワークロード チームによって制御されない一元化された機能です。

- 各機能の責任範囲と職務の分離を明確に定義します。知る必要があることと最小特権のセキュリティ原則に基づいてアクセスを制限します。
- Azure RBAC を使用して、特定のスコープでユーザー、グループ、アプリケーションにアクセス許可を割り当てます。可能な場合は、組み込みロールを使用します。
- 管理ロックを使用して、リソース、リソース グループ、またはサブスクリプションの削除または変更を防止します。
- マネージド ID を使用して Azure のリソースにアクセスする。

**Resources**

- [Azure identity and access management considerations](https://learn.microsoft.com/ja-jp/azure/well-architected/security/design-identity)

<br><br>

### WADS-9 - セキュリティ関連のリスクに確実に対処することで、予期しないセキュリティ上のリスクによって引き起こされるアプリケーションのダウンタイムとデータ損失を最小限に抑えることができます

**Category: Access & Security**

**Impact: High**

**Recommendation/Guidance**

セキュリティは、あらゆるアーキテクチャの最も重要な側面の 1 つです。貴重なデータやシステムに対する意図的な攻撃や悪用に対して、機密性、完全性、可用性を保証します。
複雑なシステムのセキュリティは、ビジネスコンテキスト、ソーシャルコンテキスト、および技術的コンテキストの理解にかかっています。システムを設計する際には、次の領域をカバーします。

- ID プロバイダー (AAD/ADFS/AD/Other) の可用性が高く、アプリケーションの可用性と復旧のターゲットと一致していることを確認します。
- すべての外部アプリケーション エンドポイントがセキュリティで保護されている。
- Virtual Network サービス エンドポイントまたは Private Link を使用してセキュリティ保護された Azure PaaS サービスへの通信。
- キーとシークレットは geo 冗長ストレージにバックアップされ、フェールオーバーの場合でも引き続き使用できます。
- キーローテーションのプロセスが自動化され、テストされていることを確認します。
- 緊急アクセスの非常用アカウントは、ID プロバイダーの障害シナリオから回復するためにテストされ、セキュリティで保護されています。

**Resources**

- [Security design principles](https://learn.microsoft.com/ja-jp/azure/well-architected/security/security-principles)

<br><br>
