+++
title = "Api Management"
description = "Best practices and resiliency recommendations for Api Management and associated resources and settings."
date = "10/20/23"
author = "DaFitRobsta"
msAuthor = "rolightn"
draft = false
+++

The presented resiliency recommendations in this guidance include Api Management and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                                                                  |   Category   | Impact |  State  | ARG Query Available |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------:|:------:|:-------:|:-------------------:|
| [APIM-1 - Migrate API Management services to Premium SKU to support Availability Zones](#apim-1---migrate-api-management-services-to-premium-sku-to-support-availability-zones) | Availability |  High  | Preview |         Yes         |
| [APIM-2 - Enable Availability Zones on Premium API Management instances](#apim-2---enable-availability-zones-on-premium-api-management-instances)                               | Availability |  High  | Preview |         Yes         |
| [APIM-3 - Upgrade to platform version stv2](#apim-3---upgrade-to-platform-version-stv2) | Availability | High | Preview | Yes |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### APIM-1 - API Management サービスを可用性ゾーンをサポートする Premium SKU に移行します

**Category: Availability**

**Impact: High**

**Guidance**

API Management インスタンスを Premium SKU にアップグレードして、Availability Zones のサポートを追加します。

**Resources**

- [Change your API Management service tier](https://learn.microsoft.com/ja-jp/azure/api-management/upgrade-and-scale#change-your-api-management-service-tier)
- [Migrate Azure API Management to availability zone support](https://learn.microsoft.com/ja-jp/azure/reliability/migrate-api-mgt)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/apim-1/apim-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### APIM-2 - Premium API Management インスタンスで可用性ゾーンを有効にします

**Category: Availability**

**Impact: High**

**Guidance**

APIM インスタンスのゾーン冗長性を有効にします。ゾーン冗長性により、API Management インスタンス (管理 API、開発者ポータル、Git 構成) のゲートウェイとコントロール プレーンが、物理的に分離されたゾーン内のデータセンター間でレプリケートされ、ゾーンの障害に対する回復性が確保されます。

**Resources**

- [Ensure API Management availability and reliability](https://learn.microsoft.com/ja-jp/azure/api-management/high-availability#availability-zones)
- [Migrate Azure API Management to availability zone support](https://learn.microsoft.com/ja-jp/azure/reliability/migrate-api-mgt)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/apim-2/apim-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### APIM-3 - Upgrade to platform version stv2

**Category: Availability**

**Impact: High**

**Guidance**

Upgrade to platform version stv2. The infrastructure associated with the API Management stv1 compute platform version will be retired effective 31 August 2024. A more current compute platform version (stv2) is already available and provides enhanced service capabilities.

**Resources**

- [Azure API Management - stv1 platform retirement (August 2024)](https://learn.microsoft.com/ja-jp/azure/api-management/breaking-changes/stv1-platform-retirement-august-2024)
- [Azure API Management compute platform](https://learn.microsoft.com/ja-jp/azure/api-management/compute-infrastructure)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/apim-3/apim-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
