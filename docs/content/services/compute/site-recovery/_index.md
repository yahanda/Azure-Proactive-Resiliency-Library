+++
title = "Azure Site Recovery"
description = "Best practices and resiliency recommendations for Azure Site Recovery and associated resources."
date = "11/23/23"
author = "pesousa"
msAuthor = "pesousa"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure Site Recovery and dependent resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                                                                      |     Category      | Impact |  State  | ARG Query Available |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [ASR-1 - Ensure static IP addresses configured in VM failover settings are available in the failover subnet](#asr-1---ensure-static-ip-addresses-configured-in-vm-failover-settings-are-available-in-the-failover-subnet)           | Disaster Recovery |  High  | Preview |         No          |
| [ASR-2 - Perform a test failover to validate the functionality and performance of the VMs in the target location](#asr-2---perform-a-test-failover-to-validate-the-functionality-and-performance-of-the-vms-in-the-target-location) | Disaster Recovery |  High  | Preview |         Yes         |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### ASR-1 - VM フェールオーバー設定で構成された静的 IP アドレスがフェールオーバー サブネットで使用可能であることを確認します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

VM フェールオーバー設定で構成された静的 IP アドレスがフェールオーバー サブネットで使用できることを確認します。フェイルオーバー中に、ターゲットサブネットのアドレス空間がソースと同じ場合、同じ静的IPアドレスがターゲットVMに割り当てられ、そうでない場合は、ターゲットサブネットで次に使用可能なIPアドレスがターゲットVMのNICアドレスとして設定されます。ターゲット IP アドレスは、VM の ネットワーク設定 で変更できます。

**Resources**

- [Setup network mapping for site recovery](https://learn.microsoft.com/ja-jp/azure/site-recovery/azure-to-azure-network-mapping#set-up-ip-addressing-for-target-vms)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/asr-1/asr-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### ASR-2 - テスト フェールオーバーを実行して、ターゲットの場所にある VM の機能とパフォーマンスを検証します

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

テスト フェールオーバーを実行して BCDR 戦略を検証し、アプリケーションがターゲット リージョンで正しく機能していることを確認します。これは、運用環境に影響を与えることなく実行できます。
テスト フェールオーバーを使用して、データの損失やダウンタイムが発生しないように、ディザスター リカバリー計画を定期的にテストします。

**Resources**

- [Run a test failover](https://learn.microsoft.com/ja-jp/azure/site-recovery/azure-to-azure-tutorial-dr-drill#run-a-test-failover)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/asr-2/asr-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
