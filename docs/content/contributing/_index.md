+++
title = "Contributing"
description = "Azure Proactive Resiliency Library (APRL) のコントリビューションガイド"
weight = 3
+++
{{< panel title="Contributions Notice" style="warning" >}} 現在、Microsoft FTE からの投稿のみを受け付けています。将来的には、これを変更する予定です。 {{< /panel >}}

Azure Proactive Resiliency Library (APRL) への貢献を検討している方は、適切な場所/ページにたどり着きました 👍

以下の指示、特に前提条件に従って、ライブラリへの貢献を開始してください。

## 推奨事項を書く

APRLの推奨事項は、Well Architected Reliability Assessmentsの提供を可能にし、加速することを目的としています。APRL の目的は、ベスト プラクティスに関する既存の Azure パブリック ドキュメントやガイダンスを置き換えることではありません。

各レコメンデーションは、お客様にとって実用的なものでなければなりません。お客様はレコメンデーションをバックログに配置でき、それを拾うエンジニアは、行う必要がある変更と、変更を行う必要がある特定のリソースを完全に明確にする必要があります。

各推奨事項には、説明的なタイトル、推奨事項の詳細を含む短いガイダンス セクション、推奨事項に関連する追加情報を提供する公開ドキュメントへのリンク、および推奨事項に準拠していないリソースを特定するためのクエリを含める必要があります。タイトルとガイダンスのセクションだけでも、CSAがリソースを評価するのに十分な情報を提供する必要があります。

推奨事項は、CSAがバックグラウンドの読み取りに多くの時間を費やすことを要求したり、解釈の余地があったり、曖昧であったりしてはなりません。WARA を提供する CSA は、限られた時間内に多数の Azure リソースをレビューしており、すべての Azure サービスの専門家ではないことに注意してください。

**例**

- 良い推奨事項: サービスに /24 サブネットを使用します
- 悪い推奨事項: サブネットのサイズを適切に設定します

すべてのベストプラクティスがAPRLの推奨に適しているわけではありません。ベストプラクティスが特定のサービス構成に関連し、ARGクエリで確認できる場合は、APRLの推奨事項として適切である可能性があります。ベスト プラクティスが、多くのサービスまたはワークロードの種類に当てはまる一般的なアーキテクチャの概念により整合している場合は、APRL WAF セクションにトピックに対処する推奨事項が既にある可能性が非常に高いです。そうでない場合は、APRL に WAF の推奨事項を追加することを検討してください。どちらも当てはまらない場合、APRLはこのコンテンツに最適な場所ではない可能性があります。

## コンテキスト/背景

前提条件と特定のセクションのコントリビューションのガイダンスに飛び込む前に、このライブラリが今後のコントリビューションを支援するためにどのように構築されているかについて、このコンテキスト/背景をよく理解してください。

この [site](https://aka.ms/aprl) は、静的サイトジェネレータである [Hugo](https://gohugo.io/) を使用して構築されており、ソースコードは [APRL GitHub リポジトリ](https://aka.ms/aprl/repo) (このサイトのヘッダーにもリンクされています) に保存され、リポジトリを介して [GitHub Pages](https://pages.github.com) でホストされています。

HugoとGitHubのページを組み合わせた理由は、ページやフォルダがたくさんあるときに使いにくいネイティブのGitHubリポジトリを使用するのではなく、ナビゲートして利用しやすいライブラリを提示できるようにするためです。また、Hugoはモバイル利用者にとっても使いやすいようにサイトを生成します。

### でも私にはHugoのスキルがありません

大丈夫です、スキルは本当に必要ありません。Hugoは、マークダウン('.md')ファイルを作成できるようにするだけで、残りの作業はサイト生成時に実行されます👍

## 前提条件

以下のセクションを読み、それに従って、APRLに貢献するための「準備完了状態」にしてください。

"準備完了状態" とは、['Azure/Azure-Proactive-Resiliency-Library' リポジトリ](https://aka.ms/aprl/repo) のフォークされたコピーがローカル コンピューターに複製され、VS Code で開かれていることを意味します。

## 開発中にAPRLのローカルコピーを実行してアクセスする

VS Code では、ターミナルを開き、次のコマンドを実行して、次のアドレス ['http://localhost:1313/Azure-Proactive-Resiliency-Library/'](http://localhost:1313/Azure-Proactive-Resiliency-Library/) を使用して、Hugo が提供するローカル Web サーバーから APRL Web サイトのコピーにアクセスできます。

{{< code lang="text" >}}cd docs
hugo server -D
{{< /code >}}

### ソフトウェア/アプリケーション

このプロジェクト/リポジトリ/ライブラリに貢献するには、次のものがインストールされている必要があります。

{{< alert style="success" >}}
「winget」を使用して、すべての前提条件を簡単にインストールできます。[以下のセクション](#winget-install-commands)を参照してください。
{{< /alert >}}

- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Visual Studio Code (VS Code)](https://code.visualstudio.com/Download)
  - Extensions:
    - `editorconfig.editorconfig`, `streetsidesoftware.code-spell-checker`, `ms-vsliveshare.vsliveshare`, `medo64.render-crlf`, `vscode-icons-team.vscode-icons`
    - VS Code では、VS Code でこのリポジトリまたはそのフォークを開くと、これらをインストールするように自動的に推奨されます。
- [Hugo Extended](https://gohugo.io/installation/)

### winget インストールコマンド

「winget」をインストールするには、[インストール手順はこちら](https://learn.microsoft.com/windows/package-manager/winget/#install-winget)に従ってください。

{{< code lang="text" >}}winget install --id 'Git.Git'
winget install --id 'Microsoft.VisualStudioCode'
winget install --id 'Hugo.Hugo.Extended'
{{< /code >}}

### その他の要件

- [A GitHub profile/account](https://github.com/join)
- ['Azure/Azure-Proactive-Resiliency-Library' リポジトリ](https://aka.ms/aprl/repo) のフォークを GitHub 組織/アカウントに作成し、マシンにローカルに複製する
  - リポジトリをフォークしてクローンする手順は [こちら](https://docs.github.com/get-started/quickstart/fork-a-repo) にあります。

## 役立つリソース

以下は、APRLに貢献する際に役立ついくつかのリソースへのリンクです。

- [Ace Documentation Theme (that we use) - Docs](https://docs.vantage-design.com/ace/)
  - [Shortcodes - e.g. Code](https://docs.vantage-design.com/ace/shortcodes/)
- [Ace Documentation Theme (that we use) - Repo](https://github.com/vantagedesign/ace-documentation)
  - [The Example Site, which is the first link above, source code](https://github.com/vantagedesign/ace-documentation/tree/master/exampleSite)
- [Hugo Quick Start](https://gohugo.io/getting-started/quick-start/)
- [Hugo Docs](https://gohugo.io/documentation/)
- [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/)

## 何かを貢献する前に行うべき手順(前提条件の後)

リポジトリのフォークが配置されているディレクトリから選択したターミナルで次のコマンドを実行します。

{{< code lang="text" >}}git checkout main
git pull
git fetch -p
git fetch -p upstream
git pull upstream main
git push
{{< /code >}}

これを行うと、アップストリーム リポジトリから最新の変更が取得され、次のコマンドを実行して 'main' から新しいブランチを作成する準備が整います。

{{< code lang="text" >}}git checkout main
git checkout -b <YOUR-DESIRED-BRANCH-NAME-HERE>
{{< /code >}}

## サービスの推奨事項ページの作成

{{< panel title="Important" style="danger" >}}
このセクションに従う前に、[何かを貢献する前に行う手順(前提条件の後)](#何かを貢献する前に行うべき手順(前提条件の後))に従っていることを確認してください。
{{< /panel >}}

実行される可能性が高い一般的なタスクは、推奨事項やサポート クエリなどを提供する新しいサービス (仮想マシンなど) を追加することです。

このタスクでは、[Hugoのアーキタイプ](https://gohugo.io/content-management/archetypes/)機能を使用して、変更して使用できる多くのテンプレートコンテンツを含む新しいサービス用のディレクトリ全体を作成できます。これは、次のコマンド 'hugo new --kind service-bundle services/<category>/<service-name' を使用して呼び出すことができます。

「service-bundle」というディレクトリアーキタイプのソースコードをリポジトリの[こちら](https://github.com/Azure/Azure-Proactive-Resiliency-Library/tree/main/docs/archetypes/service-bundle)で見ることができます。

{{< alert style="info" >}}
以下の手順では、例として仮想マシンサービスを使用します。作成したいサービスに変更してください。
{{< /alert >}}

Steps to follow:

1. In your terminal of choice run the following:
{{< code lang="text" >}}cd docs/
hugo new --kind service-bundle services/compute/virtual-machines
{{< /code >}}
2. You will now see a new folder in `content/services/compute` called `virtual-machines`
{{< code lang="text" >}}├───content
│   ├───contributing
│   └───services
│       ├───ai-ml
│       ├───compute
│       │   └───virtual-machines
│       │       └───code
│       │           ├───cm-1
│       │           └───cm-2
{{< /code >}}
3. Inside the `virtual-machines` folder you will see the following files pre-staged
{{< code lang="text" >}}│       │   └───virtual-machines
│       │       │   _index.md
│       │       │
│       │       └───code
│       │           ├───cm-1
│       │           │       cm-1.kql
│       │           │
│       │           │
│       │           └───cm-2
│       │                   cm-2.kql
{{< /code >}}
4. Open `_index.md` in VS Code and make relevant changes
    - You can copy the recommendations labelled `CM-1` or `CM-2` multiple times to create more recommendations
5. Update Azure Resource Graph queries in the `code` folder within `virtual-machines`
    - You will see there is a folder, e.g. `cm-1`, `cm-2`, per recommendation to help with file structure organization
6. Ensure you use the correct Azure resource abbreviations provided within our Cloud Adoption Framework (CAF) documentation [here](https://docs.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations). For example, use `vm` for Virtual Machines.
7. Save, commit and push your changes to your branch and repo
8. Create a [create a Pull Request](https://docs.github.com/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) into the `main` branch of the upstream repo
9. Get it merged

{{< alert style="success" >}}
[こちら](#開発中にAPRLのローカルコピーを実行してアクセスする)のガイダンスに従って、APRLウェブサイトのローカルコピーを実行することで、変更をライブで確認できることを忘れないでください。
{{< /alert >}}

## レコメンデーションの自動化標準

サービスのレコメンデーションを作成するときは、以下の基準に従ってください。

### レコメンデーションカテゴリ

各レコメンデーションには、以下の一覧から _**1 つだけ **_ カテゴリが関連付けられている必要があります。

  | Recommendation Category | Category Description |
  |:---:|:---:|
  | Application Resilience | Ensures software applications remain functional under failures or disruptions. Utilizes fault-tolerance, stateless architecture, and microservices to maintain application health and reduce downtime. |
  | Automation | Uses automated systems or scripts for routine tasks, backups, and recovery. Minimizes human intervention, thereby reducing errors and speeding up recovery processes. |
  | Availability | Focuses on ensuring services are accessible and operational. Combines basic mechanisms like backups with advanced techniques like clustering and data replication to achieve near-zero downtime. (Includes High Availability) |
  | Access & Security | Encompasses identity management, authentication, and security measures for safeguarding systems. Centralizes access control and employs robust security mechanisms like encryption and firewalls. (Includes Identity) |
  | Governance | Involves policies, procedures, and oversight for IT resource utilization. Ensures adherence to legal, regulatory, and compatibility requirements, while guiding overall system management. (Includes Compliance and Compatibility) |
  | Disaster Recovery | Involves strategies and technologies to restore systems and data after catastrophic failures. Utilizes off-site backups, recovery sites, and detailed procedures for quick recovery after a disaster. |
  | System Efficiency | Maintains acceptable service levels under varying conditions. Employs techniques like resource allocation, auto-scaling, and caching to handle changes in load and maintain smooth operation. (Includes Performance and Scalability) |
  | Monitoring | Involves constant surveillance of system health, performance, and security. Utilizes real-time alerts and analytics to identify and resolve issues quickly, aiding in faster response times. |
  | Networking | Aims to ensure uninterrupted network service through techniques like failover routing, load balancing, and redundancy. Focuses on maintaining the integrity and availability of network connections. |
  | Storage | Focuses on the integrity and availability of data storage systems. Employs techniques like RAID, data replication, and backups to safeguard against data loss or corruption. |

### Azure Resource Graph (ARG) クエリ

1. すべての ARG クエリには、クエリの上部に 2 つのコメント (1 つは "Azure Resource Graph クエリ" を示すコメント、もう 1 つは返されたクエリ結果の説明を提供するコメント) が必要です。例えば：

    ```kql
    // Azure Resource Graph Query
    // Provides a list of Azure Container Registry resources that do not have soft delete enabled
    ```

1. ARG クエリが開発中の場合、クエリには「// under-development」という 1 行が必要です。

1. ARG内で提供されるデータの制限によりレコメンデーションクエリを返すことができない場合、クエリには「// cannot-be-validated-with-arg」という1行が必要です。

1. クエリは、APRL の推奨事項に準拠していないリソースのみを返す必要があります。たとえば、Azure Container Registries の論理的な削除を有効にすることが推奨される場合、関連付けられているクエリは、論理的な削除が有効になっていない Azure Container Registry リソースのみを返す必要があります。

1. ARGクエリフォルダにファイルタイプの拡張子が「.fix」のファイルがある場合、これは現在のクエリが期待どおりに機能しないことを意味し、これをクエリを修正するための開始点として使用することを検討してください。クエリが期待どおりに動作していることを確認したら、接尾辞が '.fix' のファイルを削除してください。

1. 返される ARG クエリ列名には、次のもののみを含める必要があります。

{{< alert style="info" >}}
NOTE: 列名は、リストされている順序で、完全に一致する必要があります。
{{< /alert >}}

  | Column Name | Required | Information Returned (Example) | Description |
  |:---:|:---:|:---:|:---:|
  | recommendationId | Yes | aks-1 | The acronym of the Azure service that the query is returning results for, followed by the APRL recommendation number. |
  | name | Yes | test-aks | The resource name of the Azure resource that does not adher to the APRL recommendation. |
  | id | Yes | /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/test-resource-group/providers/Microsoft.ContainerService/managedClusters/test-aks | The resource ID of the Azure resource that does not adhere to the APRL recommendation. |
  | tags | No | {"Environment":"Test","Department":"IT"} | Any relevant tags associated to the resource that does not adhere to the APRL recommendation. |
  | param1 | No | networkProfile:kubenet | Any additional information that is necessary to provide clarification for the APRL recommendation. |
  | param2 | No | networkProfile:kubenet | Any additional information that is necessary to provide clarification for the APRL recommendation. |
  | param3 | No | networkProfile:kubenet | Any additional information that is necessary to provide clarification for the APRL recommendation. |
  | param4 | No | networkProfile:kubenet | Any additional information that is necessary to provide clarification for the APRL recommendation. |
  | param5 | No | networkProfile:kubenet | Any additional information that is necessary to provide clarification for the APRL recommendation. |

{{< alert style="info" >}}
クエリの検証に関するサポートが必要な場合は、[APRL GitHub General Question/Feedback Form](https://github.com/Azure/Azure-Proactive-Resiliency-Library/issues/new?assignees=&labels=feedback%2C+question&projects=&template=general-question-feedback----.md&title=%E2%9D%93%F0%9F%91%82+Question%2FFeedback+-+PLEASE+CHANGE+ME+TO+SOMETHING+DESCRIPTIVE) から APRL チームにお問い合わせください。
{{< /alert >}}

## サービスの推奨事項ページの更新

{{< panel title="Important" style="danger" >}}
このセクションに従う前に、[何かを貢献する前に行う手順(前提条件の後)](#何かを貢献する前に行う手順(前提条件の後))に従っていることを確認してください。
{{< /panel >}}

これは、おそらく実行される最も一般的なタスクです。

必要なのは、既存のマークダウン ('.md') ファイルを直接編集し、変更を保存し、コミットし、ステージングして、ブランチとリポジトリにプッシュすることだけです。次に、[pull request](https://docs.github.com/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)をアップストリームリポジトリの 'main'ブランチに作成すれば完了です👍

{{< alert style="success" >}}
[こちら](#開発中にAPRLのローカルコピーを実行してアクセスする)のガイダンスに従って、APRLウェブサイトのローカルコピーを実行することで、変更をライブで確認できることを忘れないでください。
{{< /alert >}}

## サービスカテゴリの作成

{{< panel title="Important" style="danger" >}}
このセクションに従う前に、[何かを貢献する前に行う手順(前提条件の後)](#何かを貢献する前に行う手順(前提条件の後))に従っていることを確認してください。
{{< /panel >}}

このタスクでは、[Hugoのアーキタイプ](https://gohugo.io/content-management/archetypes/)機能を使用して、変更して使用できる多くのテンプレートコンテンツを含む新しいサービス用のディレクトリ全体を作成できます。これは、次のコマンド 'hugo new --kind category-bundle services/<category>' を使用して呼び出すことができます。

「category-bundle」というディレクトリアーキタイプのソースコードをリポジトリの[こちら](https://github.com/Azure/Azure-Proactive-Resiliency-Library/tree/main/docs/archetypes/category-bundle)で見ることができます。

{{< alert style="info" >}}
以下の手順では、例として AAA カテゴリを使用します。作成したいカテゴリに変更してください。
{{< /alert >}}

Steps to follow:

1. In your terminal of choice run the following:
{{< code lang="text" >}}cd docs/
hugo new --kind category-bundle services/aaa
{{< /code >}}
2. You will now see a new folder in `content/services` called `aaa`
{{< code lang="text" >}}├───content
│   │   _index.md
│   │
│   ├───contributing
│   │       _index.md
│   │
│   └───services
│       │   _index.md
│       │
│       ├───aaa
│       │       _index.md
{{< /code >}}
3. Inside the `aaa` folder you will see the following file `_index.md` pre-staged
{{< code lang="text" >}}├───aaa
│       │       _index.md
{{< /code >}}
4. Open `_index.md` in VS Code and make relevant changes
5. Save, commit and push your changes to your branch and repo
6. Create a [create a Pull Request](https://docs.github.com/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) into the `main` branch of the upstream repo
7. Get it merged

{{< alert style="success" >}}
[こちら](#開発中にAPRLのローカルコピーを実行してアクセスする)のガイダンスに従って、APRLウェブサイトのローカルコピーを実行することで、変更をライブで確認できることを忘れないでください。
{{< /alert >}}

## 重要なヒント

1.Webサイトのローカルバージョンには、作成したコンテンツを反映していない不整合が表示される場合があります。
     - これが発生した場合は、<kbd>CTRL</kbd>+<kbd>C</kbd>を押してHugoローカルWebサーバを強制終了し、'docs/'ディレクトリから'hugo server -D'を実行してHugoWebサーバを再起動します
