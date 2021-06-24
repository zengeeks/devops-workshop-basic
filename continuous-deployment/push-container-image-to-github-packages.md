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

後半の演習では、 GitHub Learning Lab の [GitHub Actions: Publish to GitHub Packages](https://lab.github.com/githubtraining/github-actions:-publish-to-github-packages) を実施します。このコースには、下記の内容が含まれています。

- ベースとなるワークフローのセットアップ（CI までの処理が記載されている）
- `docker/build-push-action` アクションを用いて、Docker コンテナイメージのビルドと Container registry への push を行う
- Personal access token を用いて、Container registry からイメージを取得し起動してみる

一点だけ、演習の内容を変更する必要があります。

この演習では `docker.pkg.github.com` というレジストリに push する指示があるのですが、実はこのレジストリは、Container registry の前身である Docker registry のものです。

Container registry では `ghcr.io` に変わるので、そのように指定します。

```yml
    - name: Build container image and push it
      uses: docker/build-push-action@v1
      with:
        username: ${{github.actor}}
        password: ${{secrets.GITHUB_TOKEN}}
        # registry: docker.pkg.github.com
        registry: ghcr.io
        repository: OWNER/github-actions-for-packages/tic-tac-toe
        tag_with_sha: true
```

GitHub Learning Lab で作成したコンテナイメージを起動できましたか？ ブラウザで `http://localhost:8080` にアクセスすると、ビルドしたアプリケーション Tic Tac Toe (三目並べ) で遊ぶことができます。

さて、GitHub Learning Lab での演習が終わったら、Microsoft Learn に戻り「知識チェック」を済ませてモジュールを完了しましょう。

## 備考

### `docker/build-push-action` アクションについて

`docker/build-push-action` は、[Docker](https://github.com/docker) によって提供されている、Docker イメージをビルドし指定したレジストリに push することができるアクションです。

さまざまな機能が提供されており、柔軟なワークフローの実現をサポートします。詳しくは Marketplace や当該リポジトリをご参照ください。

なお、GitHub Learning Lab の演習では `v1` が利用されていましたが、現在は Docker [Buildx](https://github.com/docker/buildx) に対応した `v2` がリリースされており、こちらの利用が推奨されています。実際に使う際は、よくご確認の上ご利用ください。

- `docker/build-push-action`
  - [Build and push Docker images · Actions · GitHub Marketplace](https://github.com/marketplace/actions/build-and-push-docker-images)
  - [Upgrade notes - docker/build-push-action](https://github.com/docker/build-push-action/blob/master/UPGRADE.md)
  - [Handle tags and labels with `docker/metadata-action` - docker/build-push-action](https://github.com/docker/build-push-action/blob/master/docs/advanced/tags-labels.md)
- `docker/login-action`: [Docker Login · Actions · GitHub Marketplace](https://github.com/marketplace/actions/docker-login)
- `docker/metadata-action`: [Docker Metadata action · Actions · GitHub Marketplace](https://github.com/marketplace/actions/docker-metadata-action)

#### `docker/build-push-action` のアップグレードのサンプル

`docker/build-push-action@v1` のサンプル（演習から引用）
```yml
- name: Build container image and push it
  uses: docker/build-push-action@v1
  with:
    username: ${{github.actor}}
    password: ${{secrets.GITHUB_TOKEN}}
    registry: ghcr.io
    repository: OWNER/github-actions-for-packages/tic-tac-toe
    tag_with_sha: true
```

`docker/build-push-action@v2` のサンプル（演習のコードを変換）
```yml
# Docker Buildx のセットアップ
- name: Set up Docker Buildx
  uses: docker/setup-buildx-action@v1

# レジストリへログイン
- name: Login to GitHub Container registry
  uses: docker/login-action@v1
  with:
    registry: ghcr.io
    username: ${{github.actor}}
    password: ${{secrets.GITHUB_TOKEN}}

# tag_with_sha: true に相当するタグを生成
- name: Docker meta
  id: meta
  uses: docker/metadata-action@v3
  with:
    images: ghcr.io/OWNER/tic-tac-toe
    tags: |
      type=sha

# Docker イメージをビルドし push する
- name: Build container image and push it
  uses: docker/build-push-action@v2
  with:
    push: true
    tags: ${{ steps.meta.outputs.tags }}
```

### 旧 Docker registry について

旧 Docker registry に関しては、下記をご参照ください。

- [Migrating to the Container registry from the Docker registry - GitHub Docs](https://docs.github.com/en/packages/working-with-a-github-packages-registry/migrating-to-the-container-registry-from-the-docker-registry)
