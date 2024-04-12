+++
title = "Web Application Firewall"
description = "Best practices and resiliency recommendations for Web Application Firewall and associated resources."
date = "5/30/23"
author = "min-git"
msAuthor = "min-git"
draft = false
+++

The presented resiliency recommendations in this guidance include Web Application Firewall and dependent resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                                                    |  Category  | Impact |  State  | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------:|:------:|:-------:|:-------------------:|
| [WAF-1 - Review logs for Web Application Firewall on Azure Front Door for legitimate requests that are blocked](#waf-1---review-logs-for-web-application-firewall-on-azure-front-door-for-legitimate-requests-that-are-blocked)                   | Monitoring | Medium | Preview |         No          |
| [WAF-2 - Review logs for Web Application Firewall on Azure Application Gateway for legitimate requests that are blocked](#waf-2---review-logs-for-web-application-firewall-on-azure-application-gateway-for-legitimate-requests-that-are-blocked) | Monitoring | Medium | Preview |         No          |
| [WAF-3 - Monitor Web Application Firewall](#waf-3---monitor-web-application-firewall)                                                                                                                                                             | Monitoring | Medium | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### WAF-1 - Azure Front Door 上の Web アプリケーション ファイアウォールのログで、ブロックされている正当な要求を確認します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

WAFは、ブロックすべきではない正当なリクエスト(誤検知)をブロックする可能性があります。Log Analytics を使用して、過去 24 時間以内にブロックされた要求を特定できます。

**Resources**

- [Azure Web Application Firewall monitoring and logging - Access Log](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/afds/waf-front-door-monitor?pivots=front-door-standard-premium#access-logs)
- [Understanding WAF logs](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/afds/waf-front-door-tuning?pivots=front-door-standard-premium#understanding-waf-logs)
- [Web Application Firewall exclusion lists](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/ag/application-gateway-waf-configuration?tabs=portal)
- [Fixing a false positive](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/ag/web-application-firewall-troubleshoot#fixing-false-positives)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/waf-3/waf-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### WAF-2 - Azure Application Gateway 上の Web アプリケーション ファイアウォールのログで、ブロックされている正当な要求を確認します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

WAFは、ブロックすべきではない正当なリクエスト(誤検知)をブロックする可能性があります。Log Analytics を使用して、過去 24 時間以内にブロックされた要求を特定できます。

**Resources**

- [Azure Web Application Firewall Monitoring and Logging](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/ag/application-gateway-waf-metrics#logs-and-diagnostics)
- [Diagnostic logs](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/ag/web-application-firewall-logs#diagnostic-logs)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/waf-4/waf-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### WAF-3 - Web アプリケーションファイアウォールを監視します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

WAFとWAFが保護するアプリケーションの正常性を監視することは重要です。正常性の監視は、Microsoft Defender for Cloud、Azure Monitor、Azure Monitor ログとの統合によってサポートされています。

**Resources**

- [WAF monitoring](https://learn.microsoft.com/ja-jp/azure/web-application-firewall/ag/ag-overview#waf-monitoring)
- [Azure Monitor Workbook for WAF](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20WAF/Workbook%20-%20WAF%20Monitor%20Workbook)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/waf-5/waf-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
