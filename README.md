# DevOps workshop - 基礎編

このワークショップでは、GitHub Actions を用いた CI/CD の手法について学びます。

手作業は、人間の細やかな洞察を必要とする場合に向いています。しかし、すべての作業において完璧な洞察と集中力を発揮し、ミスなくこなすことはとても難しいです。中には、定型作業など、人の手を必要とせず自動化できるものもたくさんあり、これがスピード・品質を上げるための有効な施策になります。

ここでは、ソースコードを管理する GitHub 上で連携して利用できる GitHub Actions を用いた自動化について学びましょう。

## ワークショップの流れ

このワークショップでは、 [Microsoft Learn](https://docs.microsoft.com/ja-jp/learn/) や [GitHub Learning Lab](https://lab.github.com/) を用いて学習を進めます。

- 1. はじめての GitHub Actions のワークフローを作成する
- 2. Node.js 製ウェブアプリで、継続的インテグレーション (CI) を行う
- 3. Node.js 製ウェブアプリを Azure Web App へデプロイする

### ワークショップに必要な準備

| 項目 | 説明 |
|----|----|
| GitHub アカウント | 実際に GitHub にリポジトリを作って作業するため、 GitHub アカウントをご用意ください。新しく作成する方は [こちら](https://github.com/join) から作成してください。 |
| Azure アカウント | 実際にアプリケーションを Azure Web App にデプロイするため、 Azure アカウントをご用意ください。新しく作成する方は [こちら](https://azure.microsoft.com/ja-jp/free/) から作成してください。 |

また、学習に利用する Microsoft Learn は、Microsoft アカウント、組織アカウントまたはGitHub アカウントでサインインするとコンテンツの実施状況を記録できます。適宜サインインしてご利用ください。

## 1. はじめての GitHub Actions のワークフローを作成する

まず、GitHub Actions とワークフローについて学びます。こちらのモジュールを進めていきましょう。

- Microsoft Learn: [GitHub Actions を使用して開発タスクを自動化する](https://docs.microsoft.com/ja-jp/learn/modules/github-actions-automate-tasks/) (日本語)

このモジュールの演習では、 GitHub Learning Lab を利用します。初めて利用する際は、GitHub へのサインインを求められるので、ご用意したアカウントでサインインして進めてください。

GitHub Learning Lab のコースでは、日本語を選べるものもありますが、ここで利用する [GitHub Actions: Hello World](https://lab.github.com/githubtraining/github-actions:-hello-world) は英語のみのコンテンツです。

コースの学習を始めるには「Start free course」ボタンを選択して進めます。演習用のリポジトリを Public または Private のどちらに作成するか聞かれますので、特段理由がなければ、「Public」を選択し進めてください。

![](./images/github-learning-lab_github-actions-hello-world_001.png)
![](./images/github-learning-lab_github-actions-hello-world_002.png)

リポジトリの準備ができたら、「Start: Add a Dockerfile」ボタンを選択し進めましょう。

![](./images/github-learning-lab_github-actions-hello-world_003.png)

この GitHub Learning Lab では、Docker container を使ってオリジナルのアクションを作ります。実際のところ、ワークフローを組む際は既存のアクションを利用することが多いですが、どうしても必要な処理ができるアクションがない、独自処理を再利用できるようにしたい場合には有効です。

さて、 Congraturations! までたどり着けましたか？ GitHub での進捗は、GitHub Learning Lab にも反映されます。なんどでも履修できるので、不安のあるときは見返してみるとよいでしょう。

![](./images/github-learning-lab_github-actions-hello-world_004.png)

![](./images/github-learning-lab_github-actions-hello-world_005.png)

さいごに、Microsoft Learn の画面に戻り、「知識チェック」を試してみましょう。「回答を確認」で全問正解できればクリアです！「続行 >」ボタンを選択し、モジュールを終えましょう🎉

## 2. Node.js 製ウェブアプリで、継続的インテグレーション (CI) を行う

前章では初歩のワークフロー作成を体験しました。この章では、実際の開発を想定して、継続的インテグレーション（CI）ができるワークフローを作成してみましょう。

こちらのモジュールに沿って進みます。

- Microsoft Learn: [GitHub Actions を使用して継続的インテグレーション (CI) ワークフローを作成する](https://docs.microsoft.com/ja-jp/learn/modules/github-actions-ci/)

演習では、 GitHub Learning Lab の [GitHub Actions: Continuous Integration](https://lab.github.com/githubtraining/github-actions:-continuous-integration) を実施します。このコースでは、下記の内容が含まれます。

- テンプレートを使ってワークフローを作成
- アクションログの確認
- アプリケーションのテスト
- ビルド アーティファクトを利用した複数ジョブの利用
- 人によるレビューを介入させる機構
- ブランチ プロテクション

GitHub Learning Lab での演習が終わったら、Microsoft Learn に戻り「知識チェック」を済ませてモジュールを完了しましょう。

## 3. Node.js 製ウェブアプリを Azure Web App へデプロイする

CI の次は継続的デリバリー（CD）を体験してみましょう。GitHub Actions を利用しウェブアプリを Azure Web App へデプロイします。

Microsoft Learn では、[GitHub Actions を使ったアプリケーションのビルドと Azure へのデプロイ](https://docs.microsoft.com/ja-jp/learn/modules/github-actions-cd/) というモジュールがあるのですが、これは Docker を使ったものであまり一般的ではないので、ここでは下記のドキュメントを参考に学習を進めます。

- [Continuous deployment to Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/deploy-continuous-deployment?tabs=github)
  - ※ 日本語の記事の更新が追いついておらず GitHub Actions の記載がないため、英語のドキュメントを参照します。

大まかな流れは下記のとおりです。

1. ウェブアプリのコードを用意する
1. Azure Web App のリソースを作成する
1. Azure ポータルから Deployment Center を設定する

1. ウェブアプリのコードを用意する（例: Node.js）
   - [Azure で Node.js Web アプリを作成する
](https://docs.microsoft.com/ja-jp/azure/app-service/quickstart-nodejs?pivots=platform-windows) このクイックスタートの Node.js のコードを用いる
1. Web App を一つデプロイしておく
   - OS: Windows
   - Runtime stack: Node.js 14 LTS
   - SKU: Free
1. 作った直後の Web App の画面を確認する
1. Deployment Center から GitHub Actions を設定する

- [ ] TODO: Azure Web App についての簡単な説明
- [ ] [azure/webapps-deploy](https://github.com/Azure/webapps-deploy) について
- [ ] 自動作成されたシークレットについて

## 参考

- [Azure リソースの種類に推奨される省略形 - Cloud Adoption Framework | Microsoft Docs](https://docs.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations)