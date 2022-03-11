# Node.js 製ウェブアプリを GitHub Packages の Container registry で公開する

## 概要

サービスの運用にコンテナを利用しているプロジェクトを想定し、GitHub Actions を用いてコンテナイメージをレジストリに push するワークフローを作成してみましょう。

レジストリとして利用できるサービスはいくつかありますが、ここでは GitHub Packages の Container registry を利用します。

### GitHub Packages と Container registry について

**GitHub Packages** とは、様々な言語やコンテナのパッケージを、パブリックまたはプライベートに公開できるサービスです。

- [GitHub Packages とは - GitHub Docs](https://docs.github.com/ja/packages/learn-github-packages/introduction-to-github-packages)

そして、GitHub Packages の機能の一つである **Container registry** は、コンテナイメージを保存・管理することができます。現在は、Docker および Open Container Initiative (OCI) のコンテナフォーマットをサポートしています。詳細は下記ドキュメントをご参照ください。

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

このモジュールでは、 GitHub Packages のパッケージ管理機能と Container registry の両方を学習します。

パッケージ管理の例として Node.js の `npm` を用いて説明されます。npm レジストリの詳細については、下記ドキュメントをご参照ください。

- [GitHub Packages npmレジストリの利用 - GitHub Docs](https://docs.github.com/ja/packages/working-with-a-github-packages-registry/working-with-the-npm-registry)

後半の演習では、 [githubtraining/exercise-publish-package](https://github.com/githubtraining/exercise-publish-package) に従い実施します。

### [githubtraining/exercise-publish-package](https://github.com/githubtraining/exercise-publish-package) の進め方

このリポジトリが提供するサンプルの Dockerfile を用いて、イメージをビルドし、Container Registry にプッシュする流れです。

ここで、 GitHub Actions のワークフローを利用して、これらの作業をしてみましょう。

リポジトリ [githubtraining/exercise-publish-package](https://github.com/githubtraining/exercise-publish-package) の「Use this template」から、このリポジトリをテンプレートとして新しいリポジトリを作成します。

Actionsタブを開き、「New workflow」から「Publish Docker Container」テンプレートを探し、「Configure」ボタンで選択してワークフローの編集を開始します。

ワークフローを、演習で扱いやすいように２箇所書き換えます。

まず、トリガを手動で実行できるように `workflow_dispatch` に変更しましょう。

```yml
on:
  workflow_dispatch:  # ← ここ
```

つぎに、Dockerfile のディレクトリを参照させるため、 `docker/build-push-action` の引数 `context` を `sample-packages/docker` に書き換えます。

```yml
      - name: Build and push Docker image
        id: build-and-push
        uses: docker/build-push-action@main
        with:
          context: sample-packages/docker  # ← ここ
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

また、署名に関するステップは、この演習では割愛するのでコメントアウトしておきます。残しておいても問題はありません。

ワークフローをコミット＆プッシュし、正常にワークフローが終了するまで待ちます。

ワークフローが完了したら、リポジトリトップの左サイドバーに Packages にプッシュしたイメージへのリンクが表示されるので、選択しイメージを確認します。

ローカルでイメージの動作確認を行うには、こちらを参考に認証を行い操作します。

- [コンテナレジストリの利用 - GitHub Docs](https://docs.github.com/ja/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry)

さて、GitHub Learning Lab での演習が終わったら、Microsoft Learn に戻り「知識チェック」を済ませてモジュールを完了しましょう。
