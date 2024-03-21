+++
title = "Home"
description = "Azure Proactive Resiliency Library (APRL) のホームへようこそ"
weight = 1
+++

Azure Proactive Resiliency Library (APRL) のホームへようこそ。

<img src="/Azure-Proactive-Resiliency-Library/media/img/aprl-white.png" width=40%>
<br><br>

このライブラリは、Well-Architected Framework の信頼性エンゲージメント/アセスメントでお客様、パートナー様、およびフィールドが使用できるガイダンスと推奨事項のステージング領域となることを意図して構築されています。ガイダンスと推奨事項は、お客様やパートナー様とともにテストおよび検証された後、公式の [Well-Architected Framework ドキュメント](https://aka.ms/waf) に組み込まれることを目的としています。

このライブラリには、サポートする [Azure Resource Graph (ARG)](https://learn.microsoft.com/azure/governance/resource-graph/overview) クエリと、場合によっては [Azure PowerShell](https://learn.microsoft.com/powershell/azure/what-is-azure-powershell) または [Azure CLI](https://learn.microsoft.com/cli/azure/what-is-azure-cli) スクリプトも含まれており、お客様、パートナー様、およびフィールドが、ガイダンスと推奨事項に準拠している場合と準拠していない場合があるリソースを特定するのに役立ちます。これらのクエリの目的は、長期的には [Azure Advisor](https://learn.microsoft.com/azure/advisor/advisor-overview) サービスの一部にすることです。

## 始めましょう

開始するには、[Azure サービス セクション]({{< ref "services/_index.md">}}) に移動し、適切なカテゴリに移動して、Azure Resource Graph クエリ、Azure PowerShell、または 環境内の準拠リソース/非準拠リソースを検出するのに役立つ Azure CLI スクリプトのサポートに加えて、ガイダンス、推奨事項を見つけます。

{{< alert style="info" >}}

このサイトが提供する基本的な検索機能を使用して、ガイダンス、推奨事項、サポートされるクエリとスクリプトを探している Azure サービスを見つけることもできます。

{{< /alert >}}

### APRL で使用される用語の定義

APRL では、Preview (プレビュー) や Verified (検証済み) などの用語が多数使用されています。 以下の表は、分かりやすくするためにこれらの各用語の定義を示しています。

{{< table style="table-striped" >}}

| 用語 | 定義 |
| ---- | ---------- |
| Preview (プレビュー) ガイダンス | Microsoft FTE がお客様エンゲージメントに基づいて作成したガイダンスであり、内容が有効かつ正確であることを確認するために、関連する Azure 製品グループ エンジニアリング サービスのオーナーと検討中です。 |
| Verified (検証済み) ガイダンス | ガイダンスは、レビューの後、Azure 製品グループ エンジニアリング サービスのオーナーによって承認されました。 |

{{< /table >}}

## Contributing

コントリビューションガイドは [こちら]({{< ref "contributing/_index.md">}}) をご覧ください。

{{< alert style="info" >}}

このサイトは、[Ace Documentation theme](https://docs.vantage-design.com/ace/) を使用している [Hugo](https://gohugo.io/) を使用して [GithHub Pages](https://pages.github.com) に基づいて構築されていることに注意してください

{{< /alert >}}
