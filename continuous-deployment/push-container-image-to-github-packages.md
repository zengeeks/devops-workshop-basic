# Node.js 製ウェブアプリを GitHub Packages の Container registry に push する

## 概要

サービスの運用にコンテナを利用しているプロジェクトを想定し、GitHub Actions を用いてコンテナイメージをレジストリに push するワークフローを作成してみましょう。

レジストリとして利用できるサービスはいくつかありますが、ここでは GitHub Packages の Container registry を利用します。

### GitHub Packages と Container registry について

**GitHub Packages** とは、様々な言語やコンテナのパッケージを、パブリックまたはプライベートに公開できるサービスです。

- [GitHub Packages とは - GitHub Docs](https://docs.github.com/ja/packages/learn-github-packages/introduction-to-github-packages)

そして、GitHub Packages の機能の一つである **Container registry** は、コンテナイメージを保存・管理することができます。現在は、Docker および Open Container Initiative (OCI) 使用のコンテナフォーマットをサポートしています。

- [GitHub Packages コンテナレジストリの利用 - GitHub Docs](https://docs.github.com/ja/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

### その他のレジストリサービス

<details><summary>備考: Azure Container registry を使ったデプロイ</summary>
<p>

なお、Microsoft Azure の Azure Container registry と Azure Web Apps を利用したデプロイを知りたい方は、こちらの Microsoft Learn のモジュール[GitHub Actions を使ったアプリケーションのビルドと Azure へのデプロイ](https://docs.microsoft.com/ja-jp/learn/modules/github-actions-cd/) がおすすめです。

</p>
</details>

## 手順

ここでは、このモジュールに沿って進めます。

- Microsoft Learn: [GitHub Actions を活用して GitHub Packages に公開する - Learn | Microsoft Docs](https://docs.microsoft.com/ja-jp/learn/modules/github-actions-packages/)

演習では、 GitHub Learning Lab の [GitHub Actions: Publish to GitHub Packages | GitHub Learning Lab](https://lab.github.com/githubtraining/github-actions:-publish-to-github-packages) を実施します。このコースには、下記の内容が含まれています。

- ベースとなるワークフローのセットアップ（CI の処理が記載されている）
- `docker/build-push-action` アクションを用いて、Docker コンテナイメージのビルドと push を行う
- Personal access token を用いて、Container registry からイメージを取得し起動してみる

GitHub Learning Lab で作成したコンテナイメージを起動できましたか？ ブラウザで `http://localhost:8080` にアクセスすると、ビルドしたアプリケーション Tic Tac Toe (三目並べ) で遊ぶことができます。

さて、GitHub Learning Lab での演習が終わったら、Microsoft Learn に戻り「知識チェック」を済ませてモジュールを完了しましょう。

## 備考

### `docker/build-push-action` アクションについて

`docker/build-push-action` は、 Docker イメージをビルドし指定したレジストリに push することができるアクションです。

さまざまな機能が提供されており、柔軟なワークフローの実現をサポートします。詳しくは Marketplace や当該リポジトリをご参照ください。

- [docker/build-push-action - Build and push Docker images · Actions · GitHub Marketplace](https://github.com/marketplace/actions/build-and-push-docker-images)

なお、GitHub Learning Lab の演習では `v1` が利用されていましたが、現在は Docker [Buildx](https://github.com/docker/buildx) に対応した `v2` がリリースされており、こちらの利用が推奨されています。実際に使う際は、よくご確認の上ご利用ください。
