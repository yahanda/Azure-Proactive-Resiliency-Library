+++
title = "Image Templates"
description = "Best practices and resiliency recommendations for Image Templates and associated resources."
date = "8/28/23"
author = "oZakari"
msAuthor = "ztrocinski"
draft = false
+++

The presented resiliency recommendations in this guidance include Image Templates and dependent resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                              |     Category      | Impact |  State   | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:--------:|:-------------------:|
| [IT-1 - Use Generation 2 virtual machine source image](#it-1---use-generation-2-virtual-machine-source-image)               |   Availability    |  Low   | Verified |         No          |
| [IT-2 - Replicate your Image Templates to a secondary region](#it-2---replicate-your-image-templates-to-a-secondary-region) | Disaster Recovery |  Low   | Verified |         Yes         |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### IT-1 - 第 2 世代仮想マシンのソース イメージを使用します

**Category: Availability**

**Impact: Low**

**Guidance**

イメージ テンプレートを構築するときは、第 2 世代仮想マシンをサポートするソース イメージを利用します。第 2 世代 VM は、第 1 世代 VM でサポートされていない主要な機能をサポートしています。これらの機能には、メモリの増加、より大きな > 2 TB ディスクのサポート、第 1 世代 VM で使用される BIOS ベースのアーキテクチャではなく新しい UEFI ベースのブート アーキテクチャの使用により、起動とインストールの時間を短縮できる、Intel Software Guard Extensions (Intel SGX)、仮想化永続メモリ (vPMEM) が含まれます。

**Resources**

- [Generation 1 vs generation 2 virtual machines](https://learn.microsoft.com/ja-jp/azure/virtual-machines/generation-2#features-and-capabilities)

<br><br>

### IT-2 - イメージテンプレートをセカンダリリージョンにレプリケートします

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

イメージ テンプレートのデプロイに使用される Azure Image Builder サービスでは、現在、可用性ゾーンはサポートされていません。したがって、イメージテンプレートを作成するときは、セカンダリリージョン(できればプライマリリージョンのペアリージョン)にレプリケートします。これにより、リージョンの障害から迅速に回復し、イメージ テンプレートから仮想マシンを引き続きデプロイできます。

**Resources**

- [Image Template resiliency](https://learn.microsoft.com/ja-jp/azure/reliability/reliability-image-builder?toc=%2Fazure%2Fvirtual-machines%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Fbreadcrumb%2Ftoc.json#capacity-and-proactive-disaster-recovery-resiliency)
- [Azure Image Builder Supported Regions](https://learn.microsoft.com/ja-jp/azure/virtual-machines/image-builder-overview?tabs=azure-powershell#regions)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/it-2/it-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
