+++
title = "Private Endpoints"
description = "Best practices and resiliency recommendations for Private Endpoints and associated resources and settings."
date = "9/19/23"
author = "CHANGE ME TO YOUR GITHUB USERNAME"
msAuthor = "CHANGE ME TO YOUR MICROSOFT ALIAS"
draft = false
+++

The presented resiliency recommendations in this guidance include Private Endpoints and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                      |  Category       |  Impact     |  State    | ARG Query Available |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------: | :------:    | :------:  | :-----------------: |
| [PEP-1 - Resolve issues with Private Endpoints in non Succeeded connection state](#pep-1---resolve-issues-with-private-endpoints-in-non-succeeded-connection-state) | Networking      | Medium      | Preview   |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### PEP-1 - 成功以外の接続状態のプライベート エンドポイントの問題を解決します

**Category: Networking**

**Impact: Medium**

**Guidance**

プライベート エンドポイントには、静的 IP アドレスとネットワーク インターフェイス名の 2 つのカスタム プロパティがあります。これらのプロパティは、プライベート エンドポイントの作成時に設定する必要があります。状態が 成功 状態でない場合は、プライベート エンドポイントまたは関連するリソースに問題がある可能性があります。

**Resources**

- [Private endpoint connections](https://learn.microsoft.com/ja-jp/azure/private-link/manage-private-endpoint?tabs=manage-private-link-powershell#private-endpoint-connections)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/pep-1/pep-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
