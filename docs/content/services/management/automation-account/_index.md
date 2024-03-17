+++
title = "Automation Account"
description = "Best practices and resiliency recommendations for Azure Automation account and associated resources."
date = "10/18/23"
author = "poven"
msAuthor = "poven"
draft = false
+++

The presented resiliency recommendations in this guidance include Automation account and its associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Impact  |  State   | ARG Query Available |
| :------------------------------------------------ | :------: | :------: | :-----------------: |
| [AA-1 - Set up disaster recovery of Automation accounts and its dependent resources](#aa-1---set-up-disaster-recovery-of-automation-accounts-and-its-dependent-resources) | High | Preview  |         No         |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### AA-1 - Automation アカウントとその依存リソースのディザスター リカバリーを設定します

**Category: Availability**

**Impact: High**

**Guidance**

Automation アカウントと、その依存リソース (モジュール、接続、資格情報、証明書、変数、スケジュールなど) のディザスター リカバリーを設定して、リージョン全体のサービス停止またはゾーン全体の障害に対処します。ディザスター リカバリーの場合、レプリカの Automation アカウントは既にデプロイされており、プライマリ リージョンの Automation アカウントが使用できなくなった場合にフェールオーバーする準備ができている必要があります。ディザスター リカバリー戦略で、Automation アカウントと依存リソースが考慮されていることを確認します。

[PowerShell スクリプト](https://learn.microsoft.com/ja-jp/azure/automation/automation-disaster-recovery?tabs=win-hrw%2Cps-script%2Coption-one#script-to-migrate-automation-account-assets-from-one-region-to-another) を使用して Automation アカウントの資産をあるリージョンから別のリージョンに移行するか、[ARM テンプレート](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/management/overview) を使用して Automation Runbook を定義してデプロイすると、これらのテンプレートを使用して、レプリカの Automation アカウントを作成する他の Azure リージョンに同じ Runbook をデプロイできます。

**Resources**

- [Disaster recovery for Automation accounts](https://learn.microsoft.com/ja-jp/azure/automation/automation-disaster-recovery?tabs=win-hrw%2Cps-script%2Coption-one)
- [Disaster recovery scenarios for cloud and hybrid jobs](https://learn.microsoft.com/ja-jp/azure/automation/automation-disaster-recovery?tabs=win-hrw%2Cps-script%2Coption-one#scenarios-for-cloud-and-hybrid-jobs)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/aa-1/aa-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
