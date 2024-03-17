+++
title = "DB for PostgreSQL"
description = "Best practices and resiliency recommendations for Database for PostgreSQL and associated resources and settings."
date = "10/11/23"
author = "ejhenry"
msAuthor = "ejhenry"
draft = false
+++

The presented resiliency recommendations in this guidance include Database for PostgreSQL and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State            | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------:          | :-----------------: |
| [PSQL-1 - Enable HA with zone redundancy](#psql-1---enable-ha-with-zone-redundancy) | Availability | High | Preview  |         Yes         |
| [PSQL-2 - Enable custom maintenance schedule](#psql-1---enable-ha-with-zone-redundancy) | System Efficiency | High | Preview  |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### PSQL-1 - ゾーン冗長性による HA を有効化します

**Category: Availability**

**Impact: High**

**Recommendation**

フレキシブル サーバー インスタンスでゾーン冗長性を備えた HA を有効にします。ゾーン冗長高可用性は、自動フェールオーバー機能を備えた別のゾーンにスタンバイ レプリカをデプロイします。

**Resources**

- [Overview of high availability with Azure Database for PostgreSQL](https://learn.microsoft.com/ja-jp/azure/postgresql/flexible-server/concepts-high-availability)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/psql-1/psql-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### PSQL-2 - カスタムメンテナンススケジュールを有効にします

**Category: System Efficiency**

**Impact: High**

**Recommendation**

フレキシブル サーバー インスタンスでカスタム メンテナンス スケジュールを使用して、サービス更新プログラムを適用する優先時間を選択します。

**Resources**

- [Scheduled maintenance in Azure Database for PostgreSQL - Flexible Server](https://learn.microsoft.com/ja-jp/azure/postgresql/flexible-server/concepts-maintenance)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/psql-2/psql-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
