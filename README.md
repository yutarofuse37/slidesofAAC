# my-slides

[Slidev](https://sli.dev/) で作ったスライドです。`main` に push すると GitHub Pages に自動公開されます。

## ローカルで編集・発表

```bash
npm install
npm run dev
```

ブラウザで http://localhost:3030 が開きます。内容は [`slides.md`](./slides.md) を編集してください。

| コマンド | 用途 |
| --- | --- |
| `npm run dev` | プレビュー・発表 |
| `npm run build` | 静的サイトを `dist/` にビルド |
| `npm run export` | PDF / PPTX などに書き出し |

## GitHub で公開する手順

1. GitHub に新しいリポジトリを作る（例: `my-slides`）
2. このフォルダを push する:

```bash
cd ~/dev/my-slides
git remote add origin https://github.com/<あなたのユーザー名>/my-slides.git
git branch -M main
git push -u origin main
```

3. リポジトリの **Settings → Pages → Build and deployment** で **GitHub Actions** を選ぶ  
   （ワークフローの `enablement: true` で自動有効になる場合もあります）
4. Actions のデプロイが成功したら、次の URL で閲覧できます:

```
https://<あなたのユーザー名>.github.io/my-slides/
```

リポジトリ名を変えた場合も、ワークフローが `--base /リポジトリ名/` を自動で付けるのでそのままで大丈夫です。

## 編集のヒント

- スライドの区切りは Markdown の `---`
- 発表者ノートは各スライド末尾の HTML コメント `<!-- ... -->`
- テーマは `slides.md` 先頭の `theme:`（`seriph` / `default` など）
