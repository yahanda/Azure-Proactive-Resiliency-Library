+++
title = "Resource Groups"
description = "Best practices and resiliency recommendations for Resource Groups and associated resources and settings."
date = "2/22/24"
author = "edknox"
msAuthor = "edknox"
draft = false
+++

The presented resiliency recommendations in this guidance include Resource Groups and its associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                        |  Category  | Impact |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------:|:------:|:-------:|:-------------------:|
| [RG-1 - Ensure Resource Group and its Resources are located in the same Region](#rg-1---ensure-resource-group-and-its-resources-are-located-in-the-same-region) | Disaster Recovery | High | Preview |         Yes         |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### RG-1 - リソース グループとそのリソース群が同じリージョンにあることを確認します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

リソースの場所が、含まれているリソース グループの場所と一致していることを確認します。これにより、リージョンで障害が発生した場合でも、リソースを管理できます。ARM は、リソース グループ内のリソースのリソース データを格納し、リージョンが使用できない場合、このデータの更新が失敗し、リソースが実質的に読み取り専用になる可能性があります。

**Resources**

- [Azure Resource Manager Overview](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/management/overview#resource-group-location-alignment)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/rg-1/rg-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
