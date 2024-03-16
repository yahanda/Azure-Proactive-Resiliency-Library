+++
title = "4 - Deploy"
description = "Microsoft Azure Well-Architected Framework best practices and recommendations for the Reliability Stage - 4 - Deploy"
date = "9/18/23"
weight = 4
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented Microsoft Azure Well-Architected Framework recommendations in this guidance include Reliability Stage "4 - Deploy (Automation and Deployment)" and associated resources and their settings.

At this stage, the system is launched into a production environment. Proper deployment strategies, like blue-green or canary deployments, are used to minimize risks associated with releasing new versions.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                           |  Category  |  Impact   |  State    | ARG Query Available |
| :----------------------------------------------------------------------------------------------------------------------- | :--------: | :------:  | :------:  | :-----------------: |
| [WADP-1 - Avoid manual configuration to enforce consistency with Infrastructure as code](#wadp-1---avoid-manual-configuration-to-enforce-consistency-with-infrastructure-as-code)        | Automation | Medium    | Verified  |         No          |
| [WADP-2 - Validated all changes in development environments before applying them to Production](#wadp-2---validated-all-changes-in-development-environments-before-applying-them-to-production) | Automation | Medium    | Verified  |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### WADP-1 - コードとしてのインフラストラクチャとの一貫性を確保するために、手動構成を回避します

**Category: Automation**

**Impact: Medium**

**Recommendation/Guidance**

コードとしてのインフラストラクチャ (IaC) では、DevOps 手法とバージョン管理と記述モデルを使用して、ネットワーク、仮想マシン、ロード バランサー、接続トポロジなどのインフラストラクチャを定義およびデプロイします。同じソースコードで常に同じバイナリが生成されるのと同様に、IaC モデルではデプロイするたびに同じ環境が生成されます。

IaC は DevOps の重要なプラクティスであり、継続的デリバリーのコンポーネントです。IaC を使用すると、DevOps チームは、統合されたプラクティスとツールのセットと連携して、アプリケーションとそのサポート インフラストラクチャを迅速かつ確実に大規模に提供できます。

キーポイント:

- 一貫性を保つために手動設定を避ける
- 安定したテスト環境を大規模かつ迅速に提供
- 宣言型定義ファイルを使用する

**Resources**

- [Avoid manual configuration to enforce consistency](https://learn.microsoft.com/devops/deliver/what-is-infrastructure-as-code#avoid-manual-configuration-to-enforce-consistency)

<br><br>

### WADP-2 - 本番環境に適用する前に、開発環境のすべての変更を検証します

**Category: Automation**

**Impact: Medium**

**Recommendation/Guidance**

価値を継続的に提供することは、組織にとって必須の要件となっています。エンドユーザーに価値を提供するには、エラーなしで継続的にリリースする必要があります。継続的デリバリー (CD) は、ビルド、テスト、構成、およびビルド環境から運用環境への配置を自動化するプロセスです。リリース パイプラインでは、複数のテスト環境またはステージング環境を作成して、インフラストラクチャの作成を自動化し、新しいビルドをデプロイできます。連続する環境では、統合、負荷、およびユーザー受け入れテストのアクティビティの実行時間が徐々に長くなります。

**Resources**

- [Safe deployment practices](https://learn.microsoft.com/devops/operate/safe-deployment-practices)

<br><br>
