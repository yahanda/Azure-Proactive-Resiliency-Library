+++
title = "Management Groups"
description = "Best practices and resiliency recommendations for Management Groups and associated resources and settings."
date = "11/6/23"
author = "pesousa"
msAuthor = "pesousa"
draft = false
+++

The presented resiliency recommendations in this guidance include Management Groups and its associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                        |  Category  | Impact |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------:|:------:|:-------:|:-------------------:|
| [MG-1 - Subscriptions should not be placed under the Tenant Root Management Group](#mg-1---subscriptions-should-not-be-placed-under-the-tenant-root-management-group) | Governance | Medium | Preview |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### MG-1 - サブスクリプションをテナント ルート管理グループの下に配置しないでください

**Category: Governance**

**Impact: Medium**

**Guidance**

ルート管理グループは階層に組み込まれており、すべての管理グループとサブスクリプションを階層に折りたたむことができます。
ルートレベルの管理グループの下に管理グループを作成して、ホストするワークロードの種類を表します。

これらのグループは、ワークロードのセキュリティ、コンプライアンス、接続性、および機能のニーズに基づいています。このグループ化構造では、一連の Azure ポリシーを管理グループ レベルで適用できます。このグループ化構造は、同じセキュリティ、コンプライアンス、接続、および機能設定を必要とするすべてのワークロード用です。

**Resources**

- [Management group recommendations](https://learn.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/landing-zone/design-area/resource-org-management-groups#management-group-recommendations)
- [Root management group for each directory](https://learn.microsoft.com/ja-jp/azure/governance/management-groups/overview#root-management-group-for-each-directory)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/mg-1/mg-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
