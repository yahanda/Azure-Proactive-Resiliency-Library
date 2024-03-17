+++
title = "Key Vault"
description = "Best practices and resiliency recommendations for Key Vault and associated resources."
date = "5/29/23"
author = "maheshbenke"
msAuthor = "maheshbenke"
draft = false
+++

The presented resiliency recommendations in this guidance include Key Vault and associated settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                                                    |     Category      | Impact |  State  | ARG Query Available |
|:----------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:------:|:-------:|:-------------------:|
| [KV-1 - Key vaults should have soft delete enabled](#kv-1---key-vaults-should-have-soft-delete-enabled)                           | Disaster Recovery |  High  | Preview |         Yes         |
| [KV-2 - Key vaults should have purge protection enabled](#kv-2---key-vaults-should-have-purge-protection-enabled)                 | Disaster Recovery |  High  | Preview |         Yes         |
| [KV-3 - Enable Azure Private Link Service for Key vault](#kv-3---enable-azure-private-link-service-for-key-vault)                 |    Networking     |  High  | Preview |         No          |
| [KV-4 - Use separate key vaults per application per environment](#kv-4---use-separate-key-vaults-per-application-per-environment) |    Governance     |  High  | Preview |         No          |
| [KV-5 - Diagnostic logs in Key Vault should be enabled](#kv-5---diagnostic-logs-in-key-vault-should-be-enabled)                   |    Monitoring     |  Low   | Preview |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### KV-1 - Key vaults で論理的な削除を有効にする必要があります

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Key Vault の論理的な削除機能を使用すると、論理的な削除と呼ばれる、削除されたコンテナーと削除されたキー コンテナー オブジェクト (キー、シークレット、証明書など) を回復できます。論理的な削除が有効な場合、削除されたリソースとしてマークされたリソースは、指定された期間 (既定では 90 日間) 保持されます。このサービスはさらに、削除されたオブジェクトを回復し、基本的に削除を元に戻すためのメカニズムを提供します

**Resources**

- [Azure Key Vault soft-delete overview](https://learn.microsoft.com/ja-jp/azure/key-vault/general/soft-delete-overview)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/kv-1/kv-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### KV-2 - Key vaults で削除保護を有効にする必要があります

**Category: Disaster Recovery**

**Impact: High**

**Guidance**

Key vault を悪意を持って削除すると、データが完全に失われる可能性があります。組織内の悪意のある内部関係者は、key vaults を削除して消去する可能性があります。消去保護は、論理的に削除された key vaults に必須の保持期間を適用することで、インサイダー攻撃から保護します。組織内のユーザーや Microsoft は、論理的な削除の保持期間中に key vaults を消去することはできません。

**Resources**

- [Azure Key Vault purge-protection overview](https://learn.microsoft.com/ja-jp/azure/key-vault/general/soft-delete-overview#purge-protection)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/kv-2/kv-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### KV-3 - Key Vault の Azure Private Link サービスを有効にします

**Category: Networking**

**Impact: High**

**Guidance**

Azure Private Link サービスを使用すると、仮想ネットワーク内のプライベート エンドポイント経由で Azure Key Vault と Azure でホストされているお客様/パートナー サービスにアクセスできます。Azure プライベート エンドポイントは、Azure Private Link を利用したサービスにプライベートかつ安全に接続するネットワーク インターフェイスです。プライベート エンドポイントは、VNet のプライベート IP アドレスを使用して、サービスを効果的に VNet に取り込みます。サービスへのすべてのトラフィックはプライベート エンドポイント経由でルーティングできるため、ゲートウェイ、NAT デバイス、ExpressRoute または VPN 接続、パブリック IP アドレスは必要ありません。仮想ネットワークとサービス間のトラフィックは Microsoft のバックボーン ネットワークを経由するため、パブリック インターネットからの露出が排除されます。Azure リソースのインスタンスに接続して、アクセス制御の最高レベルの粒度を実現できます。

**Resources**

- [Azure Key Vault Private Link Service overview](https://learn.microsoft.com/ja-jp/azure/key-vault/general/security-features#network-security)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/kv-3/kv-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### KV-4 - 環境ごとアプリケーションごとに個別の Key vaults を使用します

**Category: Governance**

**Impact: High**

**Guidance**

Key vaults は、格納されているシークレットのセキュリティ境界を定義します。シークレットを同じ vault にグループ化すると、攻撃によって懸念事項全体のシークレットにアクセスできる可能性があるため、セキュリティ イベントの影響範囲が広がります。懸念事項間のアクセスを軽減するには、特定のアプリケーションがアクセスできるシークレットを検討し、この線引きに基づいて key vaults を分離します。アプリケーションごとに key vaults を分離することは、最も一般的な境界です。ただし、セキュリティ境界は、たとえば、関連するサービスのグループごとなど、大規模なアプリケーションに対してより詳細に設定できます。

**Resources**

- [Azure Key Vault best practices overview](https://learn.microsoft.com/ja-jp/azure/key-vault/general/best-practices#why-we-recommend-separate-key-vaults)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/kv-4/kv-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### KV-5 - Key Vault の診断ログを有効にする必要があります

**Category: Monitoring**

**Impact: Low**

**Guidance**

ログを有効にし、アラートを設定し、保持要件に従って保持します。これにより、key vaults がいつ、どのように、誰によってアクセスされたかを監視できます。

**Resources**

- [Azure Key Vault logging overview](https://learn.microsoft.com/ja-jp/azure/key-vault/general/logging?tabs=Vault)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/kv-5/kv-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
