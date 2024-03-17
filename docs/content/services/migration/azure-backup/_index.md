+++
title = "Azure Backup"
description = "Best practices and resiliency recommendations for Azure Backup and associated resources."
date = "10/11/23"
author = "poven"
msAuthor = "poven"
draft = false
+++

The presented resiliency recommendations in this guidance include Backup and associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
|
Recommendation | Category | Impact | State | ARG Query Available |
:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|--------|:--------:|:-------------------:|
| [BK-1 - Migrate from classic alerts to built-in Azure Monitor alerts for Azure Recovery Services Vaults](#bk-1---migrate-from-classic-alerts-to-built-in-azure-monitor-alerts-for-azure-recovery-services-vaults) | Monitoring | Medium | Preview | Yes |
| [BK-2 - Opt-in to Cross Region Restore for all Geo-Redundant Storage (GRS) Azure Recovery Services vaults](#bk-2---opt-in-to-cross-region-restore-for-all-geo-redundant-storage-grs-azure-recovery-services-vaults) | Disaster Recovery | Medium | Verified | Yes |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### BK-1 - Azure Recovery Services コンテナーのクラシック アラートから組み込みの Azure Monitor アラートに移行します

**Category: Monitoring**

**Impact: Medium**

**Guidance**

2026 年 3 月 31 日に、Azure Backup の Recovery Services コンテナーのクラシック アラートは廃止され、サポートされなくなります。その日付より前に、組み込みの Azure Monitor アラート ソリューションに移行します。
Azure Monitor アラートを使用すると、次のことができます。

- さまざまな通知チャネルへの通知を構成します。
- 選択したシナリオの通知を有効にします。
- バックアップ センターでアラートを大規模に監視します。
- アラートと通知をプログラムで管理します。
- バックアップを含む複数の Azure サービスに対する一貫したアラート管理。

**Resources**

- [Move to Azure monitor Alerts](https://learn.microsoft.com/ja-jp/azure/backup/move-to-azure-monitor-alerts)
- [Classic alerts retirement announcement](https://azure.microsoft.com/ja-jp/updates/transition-to-builtin-azure-monitor-alerts-for-recovery-services-vaults-in-azure-backup-by-31-march-2026/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/bk-1/bk-1.kql" >}} {{< /code >}}

{{< /collapse >}}

### BK-2 - すべての geo 冗長ストレージ (GRS) Azure Recovery Services コンテナーをリージョン間復元にオプトインします

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

リージョン間の復元を使用すると、セカンダリ リージョン (Azure ペア リージョン) に Azure VM を復元できます。このオプションを使用すると、監査またはコンプライアンスの要件を満たすための訓練を実施し、プライマリ リージョンで障害が発生した場合に VM またはそのディスクを復元できます。CRR は、GRS コンテナー専用のオプトイン機能です。

**Resources**

- [Set Cross Region Restore](https://learn.microsoft.com/ja-jp/azure/backup/backup-create-recovery-services-vault#set-cross-region-restore)
- [Azure Backup Best Practices](https://learn.microsoft.com/ja-jp/azure/backup/guidance-best-practices)
- [Minimum Role Requirements for Cross Region Restore](https://learn.microsoft.com/ja-jp/azure/backup/backup-rbac-rs-vault#minimum-role-requirements-for-azure-vm-backup)
- [Recovery Services Vault](https://azure.microsoft.com/documentation/articles/backup-azure-arm-vms-prepare/)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/bk-2/bk-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
