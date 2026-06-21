# THE BUTTON — 全人類が押す1つのボタン

全世界の人が同じ1つのカウンターを押し続けるゲーム。
ただし「破壊」はタダではできない。コツコツ押してゲージを溜めた者だけが世界を0に戻せる。
協力してレベルを上げる勢力 vs それを崩す破壊神。合言葉でグループも作れる。

## ゲームルール（案D：全部入り）
- **押す**：世界カウント +1、自分の貢献 +1、破壊ゲージ +1
- **レベル**：世界カウントが閾値（1,000 → 5,000 → 1万 → 5万 → 10万 …）を超えるとレベルアップ。全員に演出
- **破壊ゲージ**：自分で押した回数が溜まると破壊可能。必要量は `100 × 現在のレベル`。高レベルほど破壊が重い＝タダ壊し封じ
- **貢献ランキング（世界王者）**：押した回数の個人ランキング。1位は👑
- **破壊神の殿堂**：破壊した人の名前と回数を記録
- **peak記録は不滅**：壊されても過去最高到達点は残る

## 仕組み
- フロント：静的HTML 1枚（`index.html`）→ GitHub Pages
- バックエンド：Firebase Realtime Database（無料枠・サーバー管理不要）
- 同期：全クライアントにリアルタイム反映。カウントは `runTransaction` で競合なし

---

## セットアップ

### 1. firebaseConfig を貼る
Firebaseコンソール → プロジェクト設定（歯車）→「マイアプリ」→ ウェブアプリ `</>` を追加 → 表示される `firebaseConfig` を `index.html` の該当箇所に貼り替える。

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "one-button-7ab53.firebaseapp.com",
  databaseURL: "https://one-button-7ab53-default-rtdb.firebaseio.com",
  projectId: "one-button-7ab53",
};
```
`databaseURL` は必須。Realtime Databaseの画面上部のURLを使う。

### 2. データベースのルール
コンソールの Realtime Database →「ルール」タブに貼り付けて公開：

```json
{
  "rules": {
    "global": { ".read": true, ".write": true },
    "groups": {
      "$gid": { ".read": true, ".write": true }
    }
  }
}
```

### 3. GitHub Pages で公開
push 済みなら Settings → Pages で `main` / `(root)` を選択済みのはず。
公開URL：`https://chiyoko-san.github.io/one-button/`

---

## 拡張アイデア
- 効果音（押下音・破壊音・レベルアップのファンファーレ）
- 破壊予告タイマー（押すと一定時間で自動破壊。緊張感）
- 称号システム（1万貢献で「神の指」など）
- 連打ボーナス／コンボ
- 国・都道府県別カウント

## ライセンス
MIT
