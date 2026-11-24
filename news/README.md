# news/ — アプリのお知らせと新着動画

GitHub Pages（`https://kazuakitk.github.io/app-showcase/news/...`）からアプリが読む。push すれば、アプリを出し直さなくても届く（端末は 3 時間ごとに取り直す。Pages のキャッシュも 10 分ほどある）。

**このリポジトリは公開。** 下書き・社内事情は書かない（履歴に残る）。

| ファイル | 中身 | 書く人 |
|---|---|---|
| `<アプリ>.json` | アプリ別の「運営からのお知らせ」 | 手で書く |
| `videos.json` | YouTube の新着動画（全アプリ共通のポップアップ） | DevDeck の `ops/tools/video_feed.py --push`（手で書かない） |
| `videos_block.txt` | 新着動画に出さない videoId（1 行 1 本） | 手で書く |

読んでいるアプリ（2026-11-24）: `studycal`（勉強カレンダー）、`airport_code_quiz`（空港地図）、`road_quiz`（道路王）、`calendar_studio`（カレンダー工房）、`aiessay`（小論先生）、`ainovel`（文豪はしる）、`trainquiz`（駅地図）、`aihistorygame`（歴史転生）、`tabiplan`（旅プラン）、`aicontinental`（無限大陸）。共通部品は `flutter-shared` の `kazu_feed`（Flutter）と `kazu_feed/native`（Swift・Kotlin）。

## お知らせ（`<アプリ>.json`）

```json
{"version": 1, "items": [
  {"id": "2026-11-24-example", "date": "2026-11-24",
   "title": "見出し", "body": "本文（\n で改行）",
   "titleEn": "Title", "bodyEn": "Body",
   "important": false,
   "platforms": ["ios", "android", "web"],
   "minVersion": "1.2.0", "maxVersion": null,
   "startAt": "2026-11-24", "endAt": "2026-12-31",
   "linkUrl": "https://...", "linkLabel": "詳しく", "linkLabelEn": "Learn more"}
]}
```

- `id` は既読の印。一度出したら変えない
- `important: true` は起動時にダイアログで 1 回出す（その起動では新着動画は出さない）
- 日本語以外の画面には `titleEn` と `bodyEn` があるものだけ出す
- `platforms` / 版 / 期間は省略可（省略で制限なし）。リンクは https のみ
- 空港地図は、重要でない未読の最新 1 件をホームのカードにも出す
- 書いたら検査: `python3 ~/Documents/flutter/studycal/scripts/check_news_feed.py news/<アプリ>.json`

## 新着動画（`videos.json`）

- 長尺の公開動画だけ（ショートと【…の使い方】は入れない）。英語の題名は YouTube の localizations（en）から
- アプリは公開から 14 日以内の未見の 1 本を、起動時に出す（20 時間に 1 回まで。初めて開いた端末では出さない。設定で止められる）
