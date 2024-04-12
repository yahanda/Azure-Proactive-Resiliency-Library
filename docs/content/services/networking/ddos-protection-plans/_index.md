+++
title = "DDoS Protection Plans"
description = "Best practices and resiliency recommendations for DDoS Protection Plans and associated resources and settings."
date = "3/7/2024"
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented resiliency recommendations in this guidance include DDoS Protection Plans and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                      |  Category       |  Impact     |  State    | ARG Query Available |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------: | :------:    | :------:  | :-----------------: |
| [DDOS-1 - Monitor Azure DDoS Protection Plan metrics](#ddos-1---monitor-azure-ddos-protection-plan-metrics) | Access & Security      | Medium      | Preview   |         No         |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### DDOS-1 - Azure DDoS Protection プランのメトリックを監視します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

メトリック名は、さまざまなパケットタイプ、バイト数とパケット数を示し、各メトリックのタグ名の基本構造は次のとおりです。

- ドロップされたタグ名 (例: Inbound Packets Dropped DDoS): DDoS 保護システムによってドロップ/スクラビングされたパケットの数。
- 転送されたタグ名 (例: Inbound Packets Forwarded DDoS): DDoS システムによって宛先 VIP に転送されたパケットの数 (フィルター処理されなかったトラフィック)。
- タグ名なし(例:Inbound Packets DDoS):スクラビングシステムに着信したパケットの総数(ドロップおよび転送されたパケットの合計を表します)。

**Resources**

- [Monitoring Azure DDoS Protection](https://learn.microsoft.com/ja-jp/azure/ddos-protection/monitor-ddos-protection-reference)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ddos-1/ddos-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
