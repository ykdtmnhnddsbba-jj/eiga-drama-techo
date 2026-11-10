# 映画ドラマ手帖 — 引き継ぎ（HANDOFF）

最終更新: 2026-11-10

## 何を作ったか
アニメ手帖（ykdtmnhnddsbba-jj/anime-techo）と同じ構成の個人用サイト。単一 `index.html`、ビルドなし、GitHub Pages ＋ Google Drive 同期、完全無料。

| タブ | 役割 |
|---|---|
| ドラマ | 年＋クール（冬/春/夏/秋）を選ぶと、**日本のドラマ**をTMDBから取得して一覧。10段階評価・一言メモ・気になる・視聴済み・お気に入り・切った(=4点)をワンクリック。Prime/Netflix/その他配信バッジ。ドラマ名検索（海外可）。TMDBに無い作品は手動追加。検索結果のカードの「📑 シーズン別に分ける」で番組をシーズンごとの作品に分けられる（キーは `tv:<ID>:s<番号>`、特別編S0は除く） |
| 映画 | **何も自動取得しない**。タイトル検索（公開年で絞り込み可）→「☆ 気になる」で自分のリストに追加。リストは気になる/視聴済み等＋Prime・Netflixで見放題の絞り込み。見たら評価＋一言メモ。TMDBに無い映画は手動追加 |
| マイ評価 | ドラマ・映画まとめて点数別に表示 |
| 好み分析 | ドラマ／映画別。ジャンル（映画は公開年代も）から相性%を計算。他AI用プロンプト出力 |
| 設定 | Drive連携、TMDBキー、JSONバックアップ |

## 決定事項（本人の回答）
- ドラマの範囲は**日本のドラマのみ**（2026-11-10）。海外は検索で個別に追加可。
- 映画の感想は**評価＋一言メモのみ**（アニメと同じ。長文欄は作っていない）。
- 評価基準はアニメと共通: 7=完走 / 8=面白い / 9=かなり面白い / 10=神 / 切った=4。
- 映画はAmazonプライム・Netflixで観る前提（契約: Prime, Netflix）。

## データ
- ソース: TMDB（キーは**アニメ手帖と同じものを使い回し**。初回Drive連携時に `anime-techo-settings.json` から自動引き継ぎする作り＝未検証）。
- ドラマ取得: `discover/tv`（`with_origin_country=JP`, `with_original_language=ja`, `first_air_date` がクール内）。ジャンルがアニメ/ドキュメンタリー/キッズ/ニュース/リアリティ/トークのものは除外。
- 「続編・継続作品も含む」ON: `air_date` がクール内の作品（相棒のように初回放送が昔のもの）を末尾に追加。
- 配信: `/{tv|movie}/{id}/watch/providers` の JP。見放題(flatrate)でPrime/Netflix判定、その他は見放題＋無料/広告をバッジ表示。14日で再取得。
- ratings のキー: `tv:<TMDB ID>`（シリーズ丸ごと）/ `tv:<TMDB ID>:s<N>`（シーズン別） / `mv:<TMDB ID>` / 手動追加は `tv:m…` `mv:m…`。削除は `deleted:true`、マージは `updated_at` の新しい方。
- 保存: localStorage（`eigadb.*`）＋ Drive の `eiga-drama-db.json`（評価）と `eiga-drama-techo-settings.json`（TMDBキー）。

## 状態（2026-11-10 更新）
- 公開済み: https://ykdtmnhnddsbba-jj.github.io/eiga-drama-techo/ （リポジトリ ykdtmnhnddsbba-jj/eiga-drama-techo, main/root）。OAuthのリダイレクトURI登録済み。
- 本人がTMDBキー設定・クール表示（実TMDB）の動作を確認済み。ドラマ一覧・TMDB取得は実機で動作。映画検索・Drive同期・続編チェックの実機確認はこれから。
- 手帖ポータルに映画ドラマ手帖のカードを追加済み（5カード）。
- ソースの正本: GitHubリポジトリ＋このフォルダ。変更ごとに push。
- jsdom＋TMDB/Driveスタブで46項目のテストは合格。

## 次の作業候補
- 実画面での確認（TMDBの日本ドラマの網羅度、続編チェックボックスの出方）。
- 脚本家（例: 古沢良太）で探す／脚本家を好み分析に入れる（TMDB crew 取得が必要）。
- 映画の長文感想欄が欲しくなったら追加。
