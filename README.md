# slidesofAAC

[Slidev](https://sli.dev/) で作ったスライドです。`main` に push すると GitHub Pages に自動公開されます。

- リポジトリ: https://github.com/yutarofuse37/slidesofAAC
- 公開 URL（デプロイ後）: https://yutarofuse37.github.io/slidesofAAC/

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

## GitHub Pages

`main` への push で [GitHub Actions](https://github.com/yutarofuse37/slidesofAAC/actions) がビルド＆公開します。  
初回はリポジトリの **Settings → Pages → Build and deployment** で **GitHub Actions** を選んでください。

## 編集のヒント

- スライドの区切りは Markdown の `---`
- 発表者ノートは各スライド末尾の HTML コメント `<!-- ... -->`
- テーマは `slides.md` 先頭の `theme:`（`seriph` / `default` など）
