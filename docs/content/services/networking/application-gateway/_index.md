+++
title = "Application Gateway"
description = "Best practices and resiliency recommendations for Application Gateway and Web Application Firewall and associated resources."
date = "5/1/23"
author = "jimays"
msAuthor = "jimays"
draft = false
+++

The presented resiliency recommendations in this guidance include Application Gateway, Web Application Firewall and associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                               |     Category      | Impact |  State  | ARG Query Available |
|:---------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [AGW-1 - Set a minimum instance count of 2](#agw-1---set-a-minimum-instance-count-of-2)                                                      | System Efficiency |  High  | Preview |         Yes         |
| [AGW-2 - Secure all incoming connections with SSL](#agw-2---secure-all-incoming-connections-with-ssl)                                        | Access & Security |  High  | Preview |         No          |
| [AGW-3 - Enable WAF policies](#agw-3---enable-web-application-firewall-policies)                                                             | Access & Security |  High  | Preview |         Yes         |
| [AGW-4 - Use Application GW V2 instead of V1](#agw-4---use-application-gw-v2-instead-of-v1)                                                  | System Efficiency |  High  | Preview |         No          |
| [AGW-5 - Monitor and Log the configurations and traffic](#agw-5---monitor-and-log-the-configurations-and-traffic)                            |    Monitoring     | Medium | Preview |         No          |
| [AGW-6 - Use Health Probes to detect backend availability](#agw-6---use-health-probes-to-detect-backend-availability)                        |    Monitoring     | Medium | Preview |         No          |
| [AGW-7 - Deploy backends in a zone-redundant configuration](#agw-7---deploy-backends-in-a-zone-redundant-configuration)                      |   Availability    |  High  | Preview |         No          |
| [AGW-8 - Plan for backend maintenance by using connection draining](#agw-8---plan-for-backend-maintenance-by-using-connection-draining)      |    Governance     | Medium | Preview |         No          |
| [AGW-9 - Ensure Application Gateway Subnet is using a /24 subnet mask](#agw-9---ensure-application-gateway-subnet-is-using-a-24-subnet-mask) |    Networking     |  High  | Preview |         No          |

{{< /table >}}
{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### AGW-1 - 最小インスタンス数を 2 に設定します

**Category: System Efficiency**

**Impact: High**

**Guidance**

Azure Application Gateway v2 は常に高可用性方式でデプロイされ、自動スケールの構成に関係なく、既定で複数のインスタンスと共にデプロイされます。ただし、新しいインスタンスの作成には最大で 6 分から 7 分かかる場合があります。さまざまな障害モードでのダウンタイムを回避するために、最小インスタンス数を 2 に設定し、理想的にはアベイラビリティーゾーンをサポートすることをお勧めします。これにより、通常の状況では、Azure Application Gateway に常に少なくとも 2 つのインスタンスが存在することになります。そのうちの 1 つに問題が発生した場合、新しいインスタンスが作成されている間、トラフィックを処理する別のインスタンスが常に存在します。また、Auto Scaling を引き続き活用して、手動による介入を必要とせずに、トラフィック要件に基づいて動的にスケールアウトします。

**Resources**

- [Application Gateway Autoscaling Zone-Redundant](https://learn.microsoft.com/ja-jp/azure/application-gateway/application-gateway-autoscaling-zone-redundant#autoscaling-and-high-availability)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-1/agw-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-2 - すべての着信接続をSSLで保護します

**Category: Access & Security**

**Impact: High**

**Guidance**

すべての着信接続が運用サービスに HTTP/s を使用していることを確認します。エンド ツー エンドの SSL/TLS または SSL/TLS ターミネーションを使用して Application Gateway へのすべての受信接続のセキュリティを確保すると、Web サーバーとブラウザー間でやり取りされるすべてのデータがプライベートで暗号化されたままになるため、管理者とユーザーは攻撃の可能性から保護されます。

**Resources**

- [Application Gateway Security](https://learn.microsoft.com/ja-jp/azure/well-architected/services/networking/azure-application-gateway#security)
- [Application Gateway SSL Overview](https://learn.microsoft.com/ja-jp/azure/application-gateway/ssl-overview)
- [Application Gateway SSL Policy Overview](https://learn.microsoft.com/ja-jp/azure/application-gateway/application-gateway-ssl-policy-overview)
- [Application Gateway KeyVault Certs](https://learn.microsoft.com/ja-jp/azure/application-gateway/key-vault-certs)
- [Application Gateway SSL Cert Management](https://learn.microsoft.com/ja-jp/azure/application-gateway/ssl-certificate-management)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-2/agw-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-3 - Web アプリケーション ファイアウォール ポリシーを有効にします

**Category: Access & Security**

**Impact: High**

**Guidance**

アプリケーション仮想ネットワーク内の Web アプリケーション ファイアウォール (WAF) で Application Gateway を使用して、インターネットからの受信 HTTP/S トラフィックを保護します。WAFは、OWASP(Open Web Application Security Project)のコアルールセットに基づくルールを使用して、潜在的なエクスプロイトから一元的に保護します。

**Resources**

- [Well-Architected Framework Application Gateway Overview](https://learn.microsoft.com/ja-jp/azure/well-architected/services/networking/azure-application-gateway)
- [Application Gateway - Web Application Firewall](https://learn.microsoft.com/ja-jp/azure/application-gateway/features#web-application-firewall)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-3/agw-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-4 - Application GW V1 の代わりに V2 を使用します

**Category: System Efficiency**

**Impact: High**

**Guidance**

Application Gateway v1 を使用するやむを得ない理由がない限り、v2 を使用する必要があります。V2 には、自動スケーリング、静的 VIP、証明書管理のための Azure KeyVault 統合など、さらに多くの機能が組み込まれており、比較表に記載されている機能も多数あります。この更新されたバージョンを活用することで、パフォーマンスが向上し、トラフィックのルーティング方法を制御し、トラフィックに変更を加えることができます。

**Resources**

- [Application Gateway Overview V2](https://learn.microsoft.com/ja-jp/azure/application-gateway/overview-v2)
- [Application Gateway Feature Comparison Between V1 and V2](https://learn.microsoft.com/ja-jp/azure/application-gateway/overview-v2#feature-comparison-between-v1-sku-and-v2-sku)
- [Application Gateway V1 Retirement](https://azure.microsoft.com/updates/application-gateway-v1-will-be-retired-on-28-april-2026-transition-to-application-gateway-v2/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-4/agw-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-5 - 構成とトラフィックを監視してログに記録します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

ストレージ アカウント、Log Analytics、その他の監視サービスに格納できるログを有効にします。NSG を適用すると、トラフィック監査のために NSG フロー ログを有効にして保存し、Azure クラウドに流れ込むトラフィックに関する分析情報を提供できます。

**Resources**

- [Application Gateway Metrics](https://learn.microsoft.com/ja-jp/azure/application-gateway/application-gateway-metrics)
- [Application Gateway Diagnostics](https://learn.microsoft.com/ja-jp/azure/application-gateway/application-gateway-diagnostics)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-5/agw-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-6 - 正常性プローブを使用してバックエンドの可用性を検出します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

カスタム正常性プローブを使用すると、バックエンドの可用性を把握し、バックエンド サービスに何らかの影響があるかどうかを監視できます。

**Resources**

- [Application Gateway Probe Overview](https://learn.microsoft.com/ja-jp/azure/application-gateway/application-gateway-probe-overview)
- [Well-Architected Framework Application Gateway Overview](https://learn.microsoft.com/ja-jp/azure/well-architected/services/networking/azure-application-gateway)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-6/agw-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-7 - ゾーン冗長構成でバックエンドを展開します

**Category: Availability**

**Impact: High**

**Guidance**

バックエンド サービスをゾーン対応構成で展開すると、特定のゾーンがダウンした場合でも、他のゾーンにある他のサービスが引き続き使用できるため、お客様は引き続きサービスにアクセスできます。

**Resources**

- [Well-Architected Framework Application Gateway Reliability](https://learn.microsoft.com/ja-jp/azure/well-architected/services/networking/azure-application-gateway#reliability)
- [Application Gateway V2 Overview](https://learn.microsoft.com/ja-jp/azure/application-gateway/overview-v2)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-7/agw-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-8 - 接続ドレインを使用してバックエンド メンテナンスを計画します

**Category: Governance**

**Impact: Medium**

**Guidance**

接続ドレインを使用してバックエンドのメンテナンスを計画します。接続ドレインは、計画されたサービスの更新やバックエンドの正常性の問題中に、バックエンド プール メンバーを正常に削除するのに役立ちます。この設定は、バックエンド設定で有効になり、ルールの作成時にすべてのバックエンド プール メンバーに適用されます。

**Resources**

- [Application Gateway Connection Draining](https://learn.microsoft.com/ja-jp/azure/application-gateway/features#connection-draining)
- [Application Gateway Connection Draining HTTP Settings](https://learn.microsoft.com/ja-jp/azure/application-gateway/configuration-http-settings#connection-draining)

**Resource Graph Query/Scripts**
{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/agw-8/agw-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AGW-9 - Application Gateway サブネットが /24 サブネット マスクを使用していることを確認します

**Category: Networking**

**Impact: High**

**Recommendation/Guidance**

Application Gateway (Standard_v2 または WAF_v2 SKU) では、最大 125 個のインスタンスをサポートできます。Application Gateway v2 SKU のデプロイごとに /24 サブネットは必要ありませんが、強くお勧めします。これは、Application Gateway v2 に、自動スケーリングの拡張とメンテナンス アップグレードのための十分な領域を確保するためです。

**Resources**

- [Azure Application Gateway infrastructure configuration | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/application-gateway/configuration-infrastructure#size-of-the-subnet)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}
{{< code lang="sql" file="code/agw-9/agw-9.kql" >}} {{< /code >}}
{{< /collapse >}}
<br><br>
