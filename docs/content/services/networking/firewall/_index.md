+++
title = "Firewall"
description = "Best practices and resiliency recommendations for Firewall and associated resources."
date = "4/14/23"
author = "fguerri"
msAuthor = "fguerri"
draft = false
+++

The presented resiliency recommendations in this guidance include Firewall and associated Firewall settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [AFW-1 - Deploy Azure Firewall across multiple availability zones](#afw-1---deploy-azure-firewall-across-multiple-availability-zones) | Availability | High | Verified | Yes |
| [AFW-2 - Monitor Azure Firewall metrics](#afw-2---monitor-azure-firewall-metrics) | Monitoring | Medium | Verified | Yes |
| [AFW-3 - Configure DDoS Protection on the Azure Firewall VNet](#afw-3---configure-ddos-protection-on-the-azure-firewall-vnet) | Access & Security | High | Verified | Yes |
| [AFW-4 - Leverage Azure Policy inheritance model](#afw-4---leverage-azure-policy-inheritance-model) | Governance | Medium | Verified | No |
| [AFW-5 - Configure 2-4 PIPs for SNAT Port utilization](#afw-5---configure-2-4-pips-for-snat-port-utilization) | Availability | Medium | Preview | No |
| [AFW-6 - Monitor AZFW Latency Probes metric](#afw-6---monitor-azfw-latency-probes-metric) | Monitoring | Medium | Preview | No |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### AFW-1 - 複数の可用性ゾーンに Azure Firewall をデプロイします

**Category: Availability**

**Impact: High**

**Guidance**

Azure Firewall は、1 つの可用性ゾーンにデプロイする場合と、2 つ以上の可用性ゾーンにデプロイする場合で、異なる SLA を提供します。

**Resources**

- [Azure Well Architected Framework - Azure Firewall](https://learn.microsoft.com/ja-jp/azure/architecture/framework/services/networking/azure-firewall)
- [Deploy Azure Firewall across multiple availability zones](https://learn.microsoft.com/ja-jp/azure/firewall/deploy-availability-zone-powershell)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afw-1/afw-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFW-2 - Azure Firewall メトリックを監視します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

可用性とパフォーマンスの問題に関連するメトリックを監視します。具体的には、次のようになります。

- _FirewallHealth_: ファイアウォールの全体的な正常性を示します。
- _Throughput_: ファイアウォールによって処理されたスループット。スループットが文書化された制限に近づくと、アラートがトリガーされます。
- _SNATPortUtilization_: 現在使用されている送信 SNAT ポートの割合。このメトリックが 100% に近づくと、アラートがトリガーされます (その時点で、送信インターネット接続などのソース NAT 接続は失敗し始めます)。512,000 を超える SNAT ポートが必要な場合は、Azure Firewall を使用して NAT ゲートウェイをデプロイすることを検討できます。ただし、NAT ゲートウェイは現時点ではゾーン展開をサポートしていないため、ゾーン冗長ファイアウォールを使用した NAT ゲートウェイの展開は推奨されません。Azure Firewall で NAT ゲートウェイを使用するには、ゾーン ファイアウォールのデプロイが必要です。また、Azure Virtual Network NAT 統合は、セキュリティ保護付き仮想ハブ ネットワーク アーキテクチャでは現在サポートされていません。

**Resources**

- [Azure Firewall metrics supported in Azure Monitor](https://learn.microsoft.com/ja-jp/azure/azure-monitor/essentials/metrics-supported#microsoftnetworkazurefirewalls)
- [Azure Firewall performance](https://learn.microsoft.com/ja-jp/azure/firewall/firewall-performance)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afw-2/afw-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFW-3 - Azure Firewall VNet で DDoS Protection を構成します

**Category: Access & Security**

**Impact: High**

**Guidance**

DDoS 保護プランを Azure Firewall をホストしている仮想ネットワークに関連付けます。DDoS 保護プランは、DDoS 攻撃からファイアウォールを保護するための強化された軽減機能を提供します。Azure Firewall Manager は、ファイアウォール インフラストラクチャと DDoS 保護プランを作成するための統合ツールです。

**Resources**

- [Azure DDoS Protection overview](https://learn.microsoft.com/ja-jp/azure/ddos-protection/ddos-protection-overview)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afw-3/afw-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFW-4 - Azure Policy 継承モデルを活用します

**Category: Governance**

**Impact: Medium**

**Guidance**

Azure Firewall ポリシーを使用すると、ルール階層を定義し、コンプライアンスを適用できます。これは、子アプリケーション・チーム・ポリシーの上に中央の基本ポリシーをオーバーレイする階層構造を提供します。基本ポリシーの優先度が高く、子ポリシーの前に実行されます。Azure カスタム ロール定義を使用して、基本ポリシーの不注意による削除を防ぎ、サブスクリプションまたはリソース グループ内のルール コレクション グループへの選択的なアクセスを提供します。

**Resources**

- [Azure Firewall Policy hierarchy](https://learn.microsoft.com/ja-jp/azure/firewall-manager/rule-hierarchy)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afw-4/afw-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFW-5 - Configure 2-4 PIPs for SNAT Port utilization

**Category: Availability**

**Impact: Medium**

**Guidance**

Configure a minimum of two to four public IP addresses per Azure Firewall to avoid SNAT exhaustion. Azure Firewall provides SNAT capability for all outbound traffic traffic to public IP addresses. Azure Firewall provides 2,496 SNAT ports per each additional PIP.

**Resources**

- [Azure Well-Architected Framework review - Azure Firewall](https://learn.microsoft.com/en-us/azure/well-architected/service-guides/azure-firewall#recommendations)

**Resource Graphy Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afw-5/afw-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFW-6 - Monitor AZFW Latency Probes metric

**Category: Monitoring**

**Impact: Medium**

**Guidance**

Create the metric to monitor latency probes 20ms over a long period of time ( > 30mins ). When the latency probe is over a long period of time, it means the firewall instance CPUs are stressed and could possible be causing issues.

**Resources**

- [Azure Well-Architected Framework review - Azure Firewall](https://learn.microsoft.com/azure/well-architected/service-guides/azure-firewall#recommendations)
- [Azure Firewall metrics overview](https://learn.microsoft.com/azure/firewall/metrics)

**Resource Graphy Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afw-6/afw-6.kql" >}} {{< /code >}}

{{< /collapse >}}
