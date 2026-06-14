# ⛏️ マイクラ統合版 全アイテム収集チェックリスト

Minecraft 統合版（Bedrock）の全アイテム収集を、**ルームを作って身内みんなで一緒にチェック**できる Web アプリです。
HTML 1枚（`index.html`）+ Firebase Realtime Database のみで動作します。

## ✨ 主な機能

- **ルーム制** — 6桁コードのルームを作成 / コードで参加。ルームごとにチェックリストが独立
- **リアルタイム共同編集** — 誰かがチェックすると全員の画面に即反映（Firebase 同期）
- **招待リンク共有** — `#room=XXXXXX` 付きリンクをコピーして共有。開いて名前を入れるだけで参加
- **進捗バー / カテゴリ別タブ / アイテム検索**
- **プレイヤーランキング** — 誰が何個チェックしたかを全体・カテゴリ別に集計
- **編集モード** — アイテム/カテゴリの追加・名前変更・移動・削除（複数選択対応）
- 最近入ったルームの履歴からワンタップ再入室

## 🚀 使い方

1. `index.html` をブラウザで開く（または静的ホスティングに配置）
2. 名前を入力し、「ルームを作成」でルームを作る
3. ヘッダーの **🔗 招待** ボタンでリンクをコピーし、一緒に遊ぶ人に共有
4. 参加者はリンクを開いて名前を入れるだけで同じルームに参加 → みんなでチェック！

ルームコードと名前はブラウザに保存され、次回は自動で前回のルームに復帰します。
**🚪 退出** でロビーに戻れます。

## 🔧 セットアップ（自分の Firebase を使う場合）

`index.html` 内の `firebaseConfig` を自分のプロジェクトのものに置き換えてください。

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  databaseURL: "https://<your-project>-default-rtdb.firebaseio.com",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

### Realtime Database のルール

ルームは `rooms/{コード}` 配下にデータを保存します。身内用の緩い設定例:

```json
{
  "rules": {
    "rooms": {
      ".read": true,
      ".write": true
    }
  }
}
```

> ⚠️ 上記は誰でも読み書きできる設定です。公開範囲を絞りたい場合は Firebase Authentication と組み合わせてください。

## 🗂 データ構造

```
rooms/
  {ROOMCODE}/
    meta/           # { name, created, owner }
    checklist/      # { <アイテム名>: { by, at } }  ← チェック状態
    custom_data/    # 追加カテゴリ・名前変更・削除などの編集情報
```

`apiKey` が `YOUR_API_KEY` のままの場合は **デモモード**（端末内のみ・非同期）で動作します。

## 🛠 技術

- 素の HTML / CSS / JavaScript（ビルド不要・依存パッケージなし）
- Firebase Realtime Database（CDN 経由の ES Modules）
