+++
title = "Front Door"
description = "Best practices and resiliency recommendations for Front Door and associated resources."
date = "5/23/23"
author = "khushal08"
msAuthor = "khushkaviraj"
draft = false
+++

The presented resiliency recommendations in this guidance include Front Door and associated Front Door settings.

## Summary of Recommendations

The below table shows the list of resiliency recommendations for Front Door and associated resources.

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                      |     Category      | Impact |  State  | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [AFD-1 - Avoid combining Traffic Manager and Front Door](#afd-1---avoid-combining-traffic-manager-and-front-door)                                                   |    Networking     |  High  | Verified |         Yes        |
| [AFD-2 - Restrict traffic to your origins](#afd-2---restrict-traffic-to-your-origins)                                                                               | Access & Security |  High  | Verified |         No          |
| [AFD-3 - Use the latest API version and SDK version](#afd-3---use-the-latest-api-version-and-sdk-version)                                                           |    Networking     | Medium | Verified |         No          |
| [AFD-4 - Configure logs](#afd-4---configure-logs)                                                                                                                   |    Monitoring     | Medium | Verified |         No          |
| [AFD-5 - Use end-to-end TLS](#afd-5---use-end-to-end-tls)                                                                                                           |     Security      |  High  | Verified |         No          |
| [AFD-6 - Use HTTP to HTTPS redirection](#afd-6---use-http-to-https-redirection)                                                                                     | Access & Security |  High  | Verified |         No          |
| [AFD-7 - Use managed TLS certificates](#afd-7---use-managed-tls-certificates)                                                                                       | Access & Security |  High  | Verified |         No          |
| [AFD-8 - Use latest version for customer-managed certificates](#afd-8---use-latest-version-for-customer-managed-certificates)                                       | Access & Security | Medium | Verified |         No          |
| [AFD-9 - Use the same domain name on Front Door and your origin](#afd-9---use-the-same-domain-name-on-front-door-and-your-origin)                                   |    Networking     | Medium | Verified |         No          |
| [AFD-10 - Enable the WAF](#afd-10---enable-the-waf)                                                                                                                 | Access & Security | Medium | Verified |         No          |
| [AFD-11 - Disable health probes when there is only one origin in an origin group](#afd-11---disable-health-probes-when-there-is-only-one-origin-in-an-origin-group) |   Availability    |  Low   | Verified |         Yes         |
| [AFD-12 - Select good health probe endpoints](#afd-12---select-good-health-probe-endpoints)                                                                         |   Availability    | Medium | Verified |         Yes         |
| [AFD-13 - Use HEAD health probes](#afd-13---use-head-health-probes)                                                                                                 | System Efficiency | Medium | Verified |         No          |
| [AFD-14 - Use geo-filtering in Azure Front Door](#afd-14---use-geo-filtering-in-azure-front-door)                                                                   | Access & Security | Medium | Verified |         No          |
| [AFD-15 - Secure your Origin with Private Link in Azure Front Door](#afd-15---secure-your-origin-with-private-link-in-azure-front-door)                             | Access & Security | Medium | Verified |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### AFD-1 - Traffic Manager と Front Door の組み合わせは避けてください

**Category: Networking**

**Impact: High**

**Guidance**

ほとんどのソリューションでは、Front Door または Traffic Manager のいずれかを使用することをお勧めしますが、両方は使用しないでください。Azure Traffic Manager は、DNS ベースのロード バランサーです。オリジンのエンドポイントにトラフィックを直接送信します。これに対し、Azure Front Door は、クライアントに近いポイント オブ プレゼンス (PoP) で接続を終了し、配信元への有効期間の長い接続を別個に確立します。製品は動作が異なり、さまざまなユースケースを対象としています。

コンテンツのキャッシュと配信 (CDN)、TLS ターミネーション、高度なルーティング機能、または Web アプリケーション ファイアウォール (WAF) が必要な場合は、Front Door の使用を検討してください。クライアントからエンドポイントへの直接接続による単純なグローバル負荷分散の場合は、Traffic Manager の使用を検討してください。

ただし、高可用性を必要とする複雑なアーキテクチャの一部として、Azure Front Door の前に Azure Traffic Manager を配置できます。万が一、Azure Front Door が使用できなくなった場合、Azure Traffic Manager は、Azure Application Gateway やパートナー コンテンツ配信ネットワーク (CDN) などの代替宛先にトラフィックをルーティングできます。

Azure Front Door の背後に Azure Traffic Manager を配置しないでください。Azure Traffic Manager は、常に Azure Front Door の前に配置する必要があります。

**Resources**

- [Azure Load Balancing Options](https://learn.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview)
- [Azure Traffic Manager](https://learn.microsoft.com/azure/traffic-manager/traffic-manager-overview)
- [Azure Front Door](https://learn.microsoft.com/azure/frontdoor/front-door-overview)
- [Mission-critical global content delivery](https://learn.microsoft.com/ja-jp/azure/architecture/guide/networking/global-web-applications/mission-critical-content-delivery)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-1/afd-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-2 - オリジンへのトラフィックを制限します

**Category: Access & Security**

**Impact: High**

**Guidance**

Front Door の機能は、トラフィックが Front Door のみを通過する場合に最適です。Front Door 経由で送信されていないトラフィックをブロックするように配信元を構成する必要があります。

**Resources**

- [Secure traffic to Azure Front Door origins](https://learn.microsoft.com/ja-jp/azure/frontdoor/origin-security?tabs=app-service-functions&pivots=front-door-standard-premium)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-2/afd-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-3 - 最新の API バージョンと SDK バージョンを使用します

**Category: Networking**

**Impact: Medium**

**Guidance**

API、ARM テンプレート、Bicep、または Azure SDK を使用して Front Door を操作する場合は、使用可能な最新の API または SDK バージョンを使用することが重要です。APIとSDKの更新は、新しい機能が利用可能になったときに行われ、重要なセキュリティパッチとバグ修正も含まれています。

**Resources**

- [REST API Reference](https://learn.microsoft.com/rest/api/frontdoor/)
- [Client library for Java](https://learn.microsoft.com/java/api/overview/azure/resourcemanager-frontdoor-readme?view=azure-java-preview)
- [SDK for Python](https://learn.microsoft.com/python/api/overview/azure/front-door?view=azure-python)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-3/afd-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-4 - ログを設定します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

Front Door は、すべての要求に関する広範なテレメトリを追跡します。キャッシュを有効にすると、配信元サーバーがすべての要求を受信しない可能性があるため、Front Door ログを使用して、ソリューションの実行方法とクライアントへの応答方法を把握することが重要です。

**Resources**

- [Monitor metrics and logs in Azure Front Door](https://learn.microsoft.com/ja-jp/azure/frontdoor/front-door-diagnostics?pivots=front-door-standard-premium)
- [WAF logs](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/afds/waf-front-door-monitor?pivots=front-door-standard-premium#waf-logs)
- [Configure Azure Front Door logs](https://learn.microsoft.com/ja-jp/azure/frontdoor/standard-premium/how-to-logs)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-4/afd-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-5 - エンドツーエンドTLSを使用します

**Category: Security**

**Impact: High**

**Guidance**

Front Door は、クライアントからの TCP 接続と TLS 接続を終了します。次に、各ポイントオブプレゼンス(PoP)からオリジンへの新しい接続を確立します。Azure でホストされている配信元であっても、これらの各接続を TLS でセキュリティで保護することをお勧めします。このアプローチにより、転送中にデータが常に暗号化されます。

**Resources**

- [End-to-end TLS with Azure Front Door](https://learn.microsoft.com/ja-jp/azure/frontdoor/end-to-end-tls?pivots=front-door-standard-premium)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-5/afd-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-6 - HTTP から HTTPS へのリダイレクトを使用します

**Category: Access & Security**

**Impact: High**

**Guidance**

クライアントは HTTPS を使用してサービスに接続することをお勧めします。ただし、古いクライアントやベスト プラクティスを理解していない可能性のあるクライアントを許可するために、HTTP 要求を受け入れる必要がある場合があります。

HTTPS プロトコルを使用するように HTTP 要求を自動的にリダイレクトするように Front Door を構成できます。ルートで [すべてのトラフィックをリダイレクトして HTTPS を使用する] 設定を有効にする必要があります。

**Resources**

- [Create HTTP to HTTPS redirect rule](https://learn.microsoft.com/ja-jp/azure/frontdoor/front-door-how-to-redirect-https#create-http-to-https-redirect-rule)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-6/afd-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-7 - マネージド TLS 証明書を使用します

**Category: Access & Security**

**Impact: High**

**Guidance**

Front Door で TLS 証明書を管理すると、運用コストが削減され、証明書の更新を忘れることによるコストのかかる停止を回避できます。Front Door は、マネージド TLS 証明書を自動的に発行し、ローテーションします。

**Resources**

- [Configure HTTPS on an Azure Front Door custom domain using the Azure portal](https://learn.microsoft.com/ja-jp/azure/frontdoor/standard-premium/how-to-configure-https-custom-domain?tabs=powershell)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-7/afd-7.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-8 - カスタマー マネージド証明書に最新バージョンを使用します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

独自の TLS 証明書を使用する場合は、Key Vault 証明書のバージョンを "最新" に設定することを検討してください。"最新" を使用すると、新しいバージョンの証明書を使用するように Front Door を再構成したり、証明書が Front Door の環境全体に展開されるのを待ったりする必要がなくなります。

**Resources**

- [Select the certificate for Azure Front Door to deploy](https://learn.microsoft.com/ja-jp/azure/frontdoor/standard-premium/how-to-configure-https-custom-domain?tabs=powershell#select-the-certificate-for-azure-front-door-to-deploy)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-8/afd-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-9 - Front Door と配信元で同じドメイン名を使用します

**Category: Networking**

**Impact: Medium**

**Guidance**

Front Door では、受信要求の Host ヘッダーを書き換えることができます。この機能は、単一の配信元にルーティングするお客様向けの一連のカスタム ドメイン名を管理する場合に役立ちます。この機能は、Front Door と配信元でカスタム ドメイン名を構成しないようにする場合にも役立ちます。ただし、Host ヘッダーを書き換えると、要求 Cookie と URL リダイレクトが破損する可能性があります。特に、Azure App Service などのプラットフォームを使用する場合、セッション アフィニティや認証と承認などの機能が正しく動作しない可能性があります。

要求の Host ヘッダーを書き換える前に、アプリケーションが正しく動作するかどうかを慎重に検討してください。

**Resources**

- [Preserve the original HTTP host name between a reverse proxy and its back-end web application](https://learn.microsoft.com/ja-jp/azure/architecture/best-practices/host-name-preservation)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-9/afd-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-10 - WAF を有効にします

**Category: Access & Security**

**Impact: Medium**

**Guidance**

インターネットに接続するアプリケーションの場合は、Front Door Web アプリケーション ファイアウォール (WAF) を有効にし、マネージド ルールを使用するように構成することをお勧めします。WAF と Microsoft マネージド ルールを使用すると、アプリケーションはさまざまな攻撃から保護されます。

**Resources**

- [https://learn.microsoft.com/ja-jp/azure/frontdoor/web-application-firewall](https://learn.microsoft.com/ja-jp/azure/frontdoor/web-application-firewall)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-10/afd-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-11 - 配信元グループに配信元が 1 つしかない場合、正常性プローブを無効にします

**Category: Availability**

**Impact: Low**

**Guidance**

Front Door の正常性プローブは、配信元が使用できない、または異常な状況を検出するように設計されています。正常性プローブで配信元の問題が検出されると、配信元グループ内の別の配信元にトラフィックを送信するように Front Door を構成できます。

配信元が 1 つしかない場合、Front Door は、正常性プローブで異常な状態が報告された場合でも、常にその配信元にトラフィックをルーティングします。正常性プローブの状態は、Front Door の動作を変更するために何も行いません。このシナリオでは、正常性プローブには利点がないため、配信元のトラフィックを減らすために無効にする必要があります。

**Resources**

- [Health probes](https://learn.microsoft.com/ja-jp/azure/frontdoor/health-probes)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-11/afd-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-12 - 正常性プローブ エンドポイントを選択します

**Category: Availability**

**Impact: Medium**

**Guidance**

Front Door の正常性プローブに監視するように指示する場所を検討します。通常、正常性の監視用に特別に設計した Web ページまたは場所を監視することをお勧めします。アプリケーション・ロジックは、アプリケーション・サーバー、データベース、キャッシュなど、本番トラフィックの処理に必要なすべての重要なコンポーネントのステータスを考慮できます。これにより、いずれかのコンポーネントに障害が発生した場合に、Front Door はサービスの別のインスタンスにトラフィックをルーティングできます。

**Resources**

- [Health Endpoint Monitoring pattern](https://learn.microsoft.com/ja-jp/azure/architecture/patterns/health-endpoint-monitoring)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-12/afd-12.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-13 - HEAD 正常性プローブを使用します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

正常性プローブでは、GET または HEAD HTTP メソッドを使用できます。正常性プローブに HEAD メソッドを使用すると、配信元のトラフィック負荷が軽減されます。

**Resources**

- [Supported HTTP methods for health probes](https://learn.microsoft.com/ja-jp/azure/frontdoor/health-probes#supported-http-methods-for-health-probes)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-13/afd-13.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-14 - Azure Front Door でジオフィルタリングを使用します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

既定では、Azure Front Door は、要求の送信元の場所に関係なく、すべてのユーザー要求に応答します。一部のシナリオでは、国/地域ごとに Web アプリケーションへのアクセスを制限することができます。Front Door の Web アプリケーション ファイアウォール (WAF) サービスを使用すると、エンドポイント上の特定のパスに対してカスタム アクセス規則を使用してポリシーを定義し、指定した国/地域からのアクセスを許可またはブロックできます。

WAFポリシーには、一連のカスタム・ルールが含まれています。ルールは、一致条件、アクション、優先度で構成されます。一致条件では、一致変数、演算子、一致値を定義します。
地域フィルタリングルールの場合、一致変数は RemoteAddr または SocketAddr です。RemoteAddr は、通常 X-Forwarded-For 要求ヘッダーを介して送信される元のクライアント IP です。SocketAddr は、WAF が認識する送信元 IP アドレスです。ユーザーがプロキシの背後にいる場合、多くの場合、SocketAddr はプロキシ サーバーのアドレスです。この地域フィルタリング ルールの場合の演算子は GeoMatch で、値は対象の 2 文字の国/地域コードです。「ZZ」国コードまたは「不明」国は、データセット内の国にまだマッピングされていないIPアドレスをキャプチャします。一致条件に ZZ を追加して、誤検知を回避できます。GeoMatch 条件とREQUEST_URI文字列一致条件を組み合わせて、パスベースの地域フィルタリングルールを作成できます。

**Resources**

- [Geo filter WAF policy - GeoMatch](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/afds/waf-front-door-geo-filtering)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-14/afd-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AFD-15 - Azure Front Door の Private Link を使用して配信元をセキュリティで保護します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

Azure Private Link を使用すると、Azure PaaS サービスと Azure でホストされているサービスに、仮想ネットワーク内のプライベート エンドポイント経由でアクセスできます。仮想ネットワークとサービス間のトラフィックは Microsoft のバックボーン ネットワークを経由するため、パブリック インターネットへの露出がなくなります。

Azure Front Door Premium では、Private Link を使用して配信元に接続できます。配信元は、仮想ネットワークでホストすることも、Azure App Service や Azure Storage などの PaaS サービスとしてホストすることもできます。Private Link を使用すると、配信元にパブリックにアクセスする必要がなくなります。

**Resources**

- [Private link for Azure Front Door](https://learn.microsoft.com/ja-jp/azure/frontdoor/private-link)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/afd-15/afd-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
