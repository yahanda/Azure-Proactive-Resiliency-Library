+++
title = "Web App"
description = "Best practices and resiliency recommendations for Web App and associated resources."
date = "6/27/23"
author = "kunalbabre"
msAuthor = "kunalbabre"
draft = false
+++

The presented resiliency recommendations in this guidance include Web App and associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                            |Category| Impact |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------|:-:|:------:|:-------:|:-------------------:|
| [APP-1 - Enable diagnostics logging](#app-1---enable-diagnostics-logging)                                 |Monitoring|  Low   | Preview |         Yes         |
| [APP-2 - Monitor performance](#app-2---monitor-performance)                                               |Monitoring| Medium | Preview |         Yes         |
| [APP-3 - Separate web apps from web APIs](#app-3---separate-web-apps-from-web-apis)                       |System Efficiency|  Low   | Preview |         No          |
| [APP-4 - Create a separate storage account for logs](#app-4---create-a-separate-storage-account-for-logs) |System Efficiency| Medium | Preview |         No          |
| [APP-5 - Deploy to a staging slot](#app-5---deploy-to-a-staging-slot)                                     |Governance| Medium | Preview |         Yes         |
| [APP-6 - Store configuration as app settings](#app-6---store-configuration-as-app-settings)               |Application Resilience| Medium | Preview |         Yes         |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### App-1 - 診断ログを有効にします

**Category: Monitoring**

**Impact: Low**

**Guidance**

Azure App Service の診断ログを有効にすることは、監視と診断の目的で重要です。これには、アプリケーションのログ記録と Web サーバーのログ記録が含まれます。

**Resources**

- [Enable diagnostics logging for apps in Azure App Service](https://learn.microsoft.com/ja-jp/azure/app-service/troubleshoot-diagnostic-logs)

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/app-1/app-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### App-2 - パフォーマンスを監視します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

[Application Insights](https://learn.microsoft.com/ja-jp/azure/application-insights/app-insights-overview) などのパフォーマンス監視サービスを使用して、負荷がかかった状態でのアプリケーションのパフォーマンスと動作を監視します。パフォーマンス監視により、アプリケーションをリアルタイムで把握できます。これにより、問題を診断し、障害の根本原因分析を実行できます。

Azure App Service で実行されている ASP.NET、ASP.NET Core、Java、Node.jsに基づいて Web アプリケーションの監視を有効にします。以前は、アプリを手動でインストルメント化する必要がありましたが、最新の拡張機能/エージェントが既定で App Service イメージに組み込まれるようになりました。

**Resources**

- [Application Insights](https://learn.microsoft.com/ja-jp/azure/application-insights/app-insights-overview)
- [Application monitoring for Azure App Service](https://learn.microsoft.com/ja-jp/azure/azure-monitor/app/azure-web-apps)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/app-2/app-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### App-3 - Web アプリと Web API を分離します

**Category: System Efficiency**

**Impact: Low**

**Guidance**

ソリューションに Web フロントエンドと Web API の両方がある場合は、それらを個別の App Service アプリに分解することを検討してください。この設計により、ワークロードごとにソリューションを簡単に分解できます。Web アプリと API を別々の App Service プランで実行して、個別にスケーリングできます。最初はそのレベルのスケーラビリティが必要ない場合は、アプリを同じプランにデプロイし、必要に応じて後で別のプランに移動できます。

**Resources**

- [Resiliency checklist for specific Azure services](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#app-service)

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/app-3/app-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### App-4 - ログ用に別のストレージ アカウントを作成します

**Category: System Efficiency**

**Impact: Medium**

**Guidance**

ログ用に別のストレージ アカウントを作成します。ログとアプリケーション データに同じストレージ アカウントを使用しないでください。これにより、ロギングによってアプリケーションのパフォーマンスが低下するのを防ぐことができます。

**Resources**

- [Resiliency checklist](https://learn.microsoft.com/ja-jp/azure/architecture/checklist/resiliency-per-service#app-service)

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/app-4/app-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### App-5 - ステージングスロットにデプロイします

**Category: Governance**

**Impact: Medium**

**Guidance**

ステージング用のデプロイ スロットを作成します。アプリケーションの更新プログラムをステージング スロットにデプロイし、運用環境にスワップする前にデプロイを確認します。これにより、運用環境で不適切な更新が発生する可能性が低くなります。また、すべてのインスタンスが本番環境にスワップされる前にウォームアップされます。多くのアプリケーションでは、ウォームアップとコールドスタートにかなりの時間がかかります。詳細情報

前回正常起動時 (LKG) のデプロイを保持するためのデプロイ スロットの作成を検討してください。更新プログラムを運用環境にデプロイする場合は、以前の運用環境のデプロイを LKG スロットに移動します。これにより、不適切なデプロイを簡単にロールバックできます。後で問題が見つかった場合は、すぐに LKG バージョンに戻すことができます。

**Resources**

- [Set up staging environments in Azure App Service](https://learn.microsoft.com/ja-jp/azure/app-service-web/web-sites-staged-publishing)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/app-5/app-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### App-6 - 構成をアプリ設定として保存します

**Category: Application Resilience**

**Impact: Medium**

**Guidance**

アプリ設定を使用して、構成設定をアプリ設定として保持します。Resource Manager テンプレートで、または PowerShell を使用して設定を定義し、自動化されたデプロイ/更新プロセスの一部として適用できるようにし、信頼性を高めます。

**Resources**

- [Configure web apps in Azure App Service](https://learn.microsoft.com/ja-jp/azure/app-service-web/web-sites-configure)

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/app-6/app-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
