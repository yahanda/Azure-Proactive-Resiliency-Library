+++
title = "Private DNS Zones"
description = "Best practices and resiliency recommendations for Private DNS Zones and associated resources and settings."
date = "3/7/24"
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented resiliency recommendations in this guidance include Private DNS Zones and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [PVDNSZ-1 - Protect private DNS zones and records](#pvdnsz-1---protect-private-dns-zones-and-records) | Access & Security | Medium | Preview | No |
| [PVDNSZ-2 - Monitor Private DNS Zones health and set up alerts](#pvdnsz-2---monitor-private-dns-zones-health-and-set-up-alerts) | Monitoring | Low | Preview | No |
| [PVDNSZ-3 - Make sure Production and DR zones have equivalent entries for workloads and resources that will be failed over](#pvdnsz-3---make-sure-production-and-dr-zones-have-equivalent-entries-for-workloads-and-resources-that-will-be-failed-over) | Governance | Medium | Preview | No |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### PVDNSZ-1 - プライベート DNS ゾーンとレコードを保護します

**Category: Access & Security**

**Impact: Medium**

**Guidance**

プライベート DNS ゾーンとレコードは重要なリソースです。DNS ゾーンまたは 1 つの DNS レコードを削除すると、サービスが停止する可能性があります。DNS ゾーンとレコードは、承認されていない変更や偶発的な変更から保護されることが重要です。プライベート DNS ゾーン共同作成者ロールは、プライベート DNS リソースを管理するための組み込みロールです。ユーザーまたはグループに適用されるこのロールにより、プライベート DNS リソースを管理できます。

**Resources**

- [Protecting private DNS Zones and Records - Azure DNS](https://learn.microsoft.com/ja-jp/azure/dns/dns-protect-private-zones-recordsets)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/pvdnsz-1/pvdnsz-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### PVDNSZ-2 - プライベート DNS ゾーンの正常性の監視とアラートを設定します

**Category: Monitoring**

**Impact: Low**

**Guidance**

プライベート DNS ゾーンに含まれるレコードは、インターネットから解決できません。プライベート DNS ゾーンに対する DNS 解決は、そのゾーンにリンクされている仮想ネットワークからのみ機能します。仮想ネットワーク リンクを作成することで、プライベート DNS ゾーンを 1 つ以上の仮想ネットワークにリンクできます。また、自動登録機能を有効にして、仮想ネットワークにデプロイされる仮想マシンの DNS レコードのライフ サイクルを自動的に管理することもできます。

**Resources**

- [Scenarios for Azure Private DNS zones](https://learn.microsoft.com/ja-jp/azure/dns/private-dns-scenarios)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/pvdnsz-2/pvdnsz-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### PVDNSZ-3 - 本番ゾーンと DR ゾーンに、フェールオーバーされるワークロードとリソースの同等のエントリがあることを確認します

**Category: Governance**

**Impact: Medium**

**Guidance**

Azure プライベート DNS は、カスタム DNS ソリューションを追加することなく、仮想ネットワーク内のドメイン名を管理および解決するための、信頼性が高く安全な DNS サービスを提供します。プライベート DNS ゾーンを使用すると、現在使用可能な Azure 提供の名前ではなく、独自のカスタム ドメイン名を使用できます。プライベート DNS ゾーンに含まれるレコードは、インターネットから解決できません。プライベート DNS ゾーンに対する DNS 解決は、そのゾーンにリンクされている仮想ネットワークからのみ機能します。仮想ネットワーク リンクを作成することで、プライベート DNS ゾーンを 1 つ以上の仮想ネットワークにリンクできます。また、自動登録機能を有効にして、仮想ネットワークにデプロイされる仮想マシンの DNS レコードのライフ サイクルを自動的に管理することもできます。

**Resources**

- [Scenarios for Azure Private DNS zones](https://learn.microsoft.com/ja-jp/azure/dns/private-dns-scenarios)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/pvdnsz-3/pvdnsz-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
