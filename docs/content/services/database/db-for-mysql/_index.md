+++
title = "DB for MySQL"
description = "Best practices and resiliency recommendations for Db for Mysql and associated resources and settings."
date = "2/26/24"
author = "ejhenry"
msAuthor = "erhenry"
draft = false
+++

The presented resiliency recommendations in this guidance include DB for MySQL and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |                                Category                                 |     Impact      |      State       | ARG Query Available |
|:--------------------------------------------------|:-----------------------------------------------------------------------:|:---------------:|:----------------:|:-------------------:|
| [MYSQL-1 - Enable HA with zone redundancy](#mysql-1---enable-ha-with-zone-redundancy) | Availability | High | Verified |         Yes         |
| [MYSQL-2 - Enable custom maintenance schedule](#mysql-2---enable-custom-maintenance-schedule) |     System Efficiency      | High | Verified |         Yes          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### MYSQL-1 - ゾーン冗長性による HA を有効化します

**Category: Availability**

**Impact: High**

**Guidance**

フレキシブル サーバー インスタンスでゾーン冗長性を備えた HA を有効にします。ゾーン冗長高可用性は、自動フェールオーバー機能を備えた別のゾーンにスタンバイ レプリカをデプロイします。

**Resources**

- [High availability concepts in Azure Database for MySQL - Flexible Server](https://learn.microsoft.com/ja-jp/azure/mysql/flexible-server/concepts-high-availability)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/mysql-1/mysql-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### MYSQL-2 - カスタムメンテナンススケジュールを有効にします

**Category: System Efficiency**

**Impact: High**

**Guidance**

フレキシブル サーバー インスタンスでカスタム メンテナンス スケジュールを使用して、サービス更新プログラムを適用する優先時間を選択します。

**Resources**

- [Scheduled maintenance in Azure Database for MySQL - Flexible Server](https://learn.microsoft.com/ja-jp/azure/mysql/flexible-server/concepts-maintenance)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/mysql-2/mysql-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
