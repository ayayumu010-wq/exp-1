# step1.md — 現在のプロジェクトの仕様と各ファイルの解説

## 0. このプロジェクトは何か

- 名前: `exp-1`
- 目的: 「Japan Move & Setup Cost Calculator（日本への引っ越し・初期費用計算機）」のウェブサイトを作る土台です。
- 現状: 画面には **「Coming Soon」と書かれたタイトルだけ** が表示されます。計算機の機能はまだありません。
- 技術: Next.js 16 + React 19 + TypeScript + Tailwind CSS 4
- 公開方法: 静的書き出し（Static Export）でHTMLを作り、Cloudflare Pagesに無料で置く予定です。

---

## 1. フォルダ全体の地図

```
exp-1/
├─ src/app/          ← 画面を作るメインの場所（実際に編集する所）
│   ├─ page.tsx      ← トップページの中身
│   ├─ layout.tsx    ← 全ページ共通の枠（HTMLの外側）
│   ├─ globals.css   ← 全体の色やフォントの設定
│   └─ favicon.ico   ← タブに出る小さなアイコン
├─ public/           ← そのまま公開される画像置き場
├─ next.config.ts    ← Next.jsの設定（静的書き出しをON）
├─ package.json      ← 使う部品と、コマンドの一覧
├─ package-lock.json ← 部品のバージョンの記録
├─ tsconfig.json     ← TypeScriptの設定
├─ eslint.config.mjs ← コードの書き方チェックの設定
├─ next-env.d.ts     ← Next.jsの型の自動生成ファイル
├─ .gitignore        ← Gitに入れないファイルの指定
├─ AGENTS.md         ← AIへの注意書き（日本語化済み）
├─ README.md         ← プロジェクトの説明書（日本語化済み）
├─ out/              ← 【自動生成】ビルドで作られた公開用サイト
├─ .next/            ← 【自動生成】Next.jsの作業用キャッシュ
└─ node_modules/     ← 【自動生成】ダウンロードされた部品の山
```

> 【自動生成】の3つは自分で編集しません。消しても `npm install` や `npm run build` で作り直せます。

---

## 2. 各ファイルの解説

### 2-1. `src/app/page.tsx`（トップページ）

```tsx
export default function Home() {
  return (
    <main className="flex min-h-screen items-center justify-center bg-white p-6">
      <h1 className="text-center text-3xl font-semibold text-gray-900">
        Japan Move &amp; Setup Cost Calculator (Coming Soon)
      </h1>
    </main>
  );
}
```

- サイトの `/`（トップ）に表示される内容です。
- `className` はTailwind CSSの書き方です。
  - `flex` `items-center` `justify-center`: 中身を画面の真ん中に置く
  - `min-h-screen`: 画面いっぱいの高さ
  - `bg-white`: 背景は白
  - `text-3xl font-semibold text-gray-900`: 大きめ・太め・濃いグレーの文字
- `&amp;` は文字の「&」を表すHTMLの書き方です。
- 計算機を作るときは、主にこのファイルに書き足していきます。

### 2-2. `src/app/layout.tsx`（全ページ共通の枠）

- すべてのページを包む `<html>` と `<body>` を定義します。
- `Geist` と `Geist_Mono` というフォントを読み込み、CSS変数として使えるようにしています。
- `metadata` は **検索結果に出る「タイトル」と「説明文」** です。
  - 今は初期値の `"Create Next App"` のままです。
  - **SEOのため、近いうちに書き換えるのがおすすめです。**
- `lang="en"` は「このページは英語」という宣言です。英語のサイトならこのままでOKです。

### 2-3. `src/app/globals.css`（全体の見た目）

- `@import "tailwindcss";` でTailwindを有効にします。
- `:root` で背景色(`--background`)と文字色(`--foreground`)を決めています。
- `@media (prefers-color-scheme: dark)` で、ダークモードの人には黒い背景にします。
- `@theme inline { ... }` はTailwind 4専用の書き方です。
  - ビルド時の「Unknown at rule: @theme」という警告は、これをCSSの解析機能が知らないだけです。**無視してOKです。**
- 注意: 今のトップページは `bg-white` を直接指定しているため、ダークモードでも白背景になります。

### 2-4. `next.config.ts`（Next.jsの設定）

```ts
const nextConfig: NextConfig = {
  output: "export",
  images: { unoptimized: true },
};
```

- `output: "export"`: ビルド時に `out/` フォルダへ静的なHTMLを書き出します。
- `images: { unoptimized: true }`: 静的書き出しでは画像の自動最適化が使えないため、オフにしています。

### 2-5. `package.json`（部品とコマンド）

**コマンド（`npm run ◯◯`）**

| コマンド | 意味 |
|---|---|
| `npm run dev` | 開発用サーバーを起動（http://localhost:3000 で確認） |
| `npm run build` | 公開用のサイトを `out/` に作る |
| `npm run start` | ビルド結果をサーバーで起動する（静的書き出しでは基本使わない） |
| `npm run lint` | コードの書き方をチェックする |

**主な部品**

- `next` / `react` / `react-dom`: サイトを作る本体
- `tailwindcss`: 見た目を整える道具
- `typescript`: 間違いを事前に見つけてくれる言語
- `eslint`: コードの書き方チェッカー

### 2-6. `public/`

- `next.svg` `vercel.svg` など、最初から入っているサンプル画像です。
- 今のページでは使っていないため、**消しても問題ありません。**
- ここに置いたファイルは `/ファイル名` でそのまま公開されます。

### 2-7. `out/`（ビルドで生成された公開用サイト）

- `index.html`: 完成したトップページ
- `404.html`: ページが見つからないとき用
- `_next/`: CSSやJavaScriptの部品
- **Cloudflare Pagesには、このフォルダの中身が公開されます。**
- `.gitignore` で除外されている場合があり、GitHubには上げず、Cloudflare側でビルドさせます。

---

## 3. 作業の流れ（全体像）

```
① コードを編集（src/app/page.tsx など）
        ↓
② npm run dev で見た目を確認
        ↓
③ npm run build で out/ ができるか確認
        ↓
④ GitHubにプッシュ
        ↓
⑤ Cloudflare Pagesが自動でビルドして公開
```

---

## 4. 今後のおすすめ作業（次のステップ）

1. `layout.tsx` の `title` と `description` を、サイトに合った内容に変更する（SEO対策）
2. `page.tsx` に計算機の入力欄と計算結果を作る（入力にはReactの `useState` を使う想定。その場合ファイルの先頭に `"use client";` が必要です）
3. 使っていない `public/` のサンプル画像を整理する
4. GitHubにプッシュ → Cloudflare Pagesに接続する
