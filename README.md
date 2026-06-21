# THE BUTTON — 全人類が押す1つのボタン

全世界の人が同じ1つのカウンターを押し続けるゲーム。
誰でも「破壊」ボタンで積み上がった数字を0に戻せる。協力 vs 破壊。
合言葉でグループを作れば、仲間内だけのボタンも持てる。

## 仕組み
- フロント: 静的HTML 1枚（`index.html`）→ GitHub Pages で公開
- バックエンド: Firebase Realtime Database（無料枠で十分・サーバー管理不要）
- 同期: 全クライアントにリアルタイム反映。カウントは `runTransaction` で競合なし

---

## セットアップ手順

### 1. Firebase プロジェクトを作る
1. https://console.firebase.google.com/ で「プロジェクトを追加」
2. 左メニュー「構築」→「Realtime Database」→「データベースを作成」
3. ロケーションを選び、**テストモード**で開始（後でルールを締める）

### 2. 設定をコピー
1. プロジェクト設定（歯車）→「マイアプリ」→ ウェブアプリ `</>` を追加
2. 表示される `firebaseConfig` を、`index.html` の以下の部分に貼り替える：

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT",
};
```

> `databaseURL` は必須。Realtime Database の画面上部に表示される URL を使う。

### 3. データベースのルール（推奨）
テストモードのままだと誰でも全データを書き換えられる。最低限こうしておく：

```json
{
  "rules": {
    "global": {
      ".read": true,
      ".write": true
    },
    "groups": {
      "$gid": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

（このゲームは「誰でも押せる・誰でも壊せる」が仕様なので write は開放でOK。荒らし対策をするなら App Check や匿名認証を足す。）

---

## GitHub Pages で公開

```bash
git init
git add .
git commit -m "THE BUTTON"
git branch -M main
git remote add origin https://github.com/＜ユーザー名＞/the-button.git
git push -u origin main
```

1. GitHub のリポジトリ →「Settings」→「Pages」
2. Source を `main` ブランチ / `root` に設定して Save
3. 数分後 `https://＜ユーザー名＞.github.io/the-button/` で公開

---

## カスタムのアイデア
- 破壊者ランキング（`destroyers/` に名前と回数を積む）
- 押した瞬間の演出（画面シェイク・効果音）
- レベル制（1000ごとにファンファーレ）
- 地域別カウント（IPや手動選択で国・都道府県別）

## ライセンス
MIT — 好きにどうぞ。
