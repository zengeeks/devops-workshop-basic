# Node.js 製ウェブアプリを GitHub Packages の Container Registry に push する

（作成中）

## 概要

サービスの運用にコンテナを利用しているプロジェクトを想定し、GitHub Actions を用いてコンテナイメージをレジストリに push するワークフローを作成してみましょう。

レジストリとして利用できるサービスはいくつかありますが、ここでは GitHub Packages の Container Registry を利用します。

### GitHub Packages と Container Registry について

**GitHub Packages** とは、様々な言語やコンテナのパッケージを、パブリックまたはプライベートに公開できるサービスです。

- [GitHub Packages とは - GitHub Docs](https://docs.github.com/ja/packages/learn-github-packages/introduction-to-github-packages)

そして、GitHub Packages の機能の一つである **Container Registry** は、コンテナイメージを保存・管理することができます。現在は、Docker および Open Container Initiative (OCI) 使用のコンテナフォーマットをサポートしています。

- [GitHub Packages コンテナレジストリの利用 - GitHub Docs](https://docs.github.com/ja/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

### その他のレジストリサービス

<details><summary>備考: Azure Container Registry を使ったデプロイ</summary>
<p>

なお、Microsoft Azure の Azure Container Registry と Azure Web Apps を利用したデプロイを知りたい方は、こちらの Microsoft Learn のモジュール[GitHub Actions を使ったアプリケーションのビルドと Azure へのデプロイ](https://docs.microsoft.com/ja-jp/learn/modules/github-actions-cd/) がおすすめです。

</p>
</details>

## 手順

ここでは、このモジュールに沿って進めます。

- Microsoft Learn: [GitHub Actions を活用して GitHub Packages に公開する - Learn | Microsoft Docs](https://docs.microsoft.com/ja-jp/learn/modules/github-actions-packages/)

演習では、 GitHub Learning Lab の [GitHub Actions: Publish to GitHub Packages | GitHub Learning Lab](https://lab.github.com/githubtraining/github-actions:-publish-to-github-packages) を実施します。このコースには、下記の内容が含まれています。

- 