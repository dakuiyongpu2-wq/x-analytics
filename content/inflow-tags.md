# 流入タグ台帳（inflow tags）

LINE誘導ページ `line/index.html` に付ける流入元タグ（`ref`）の一覧と命名ルール。
リンクを新しく発行したら、必ずこの表に1行足すこと。

**ベースURL**

```
https://dakuiyongpu2-wq.github.io/x-analytics/line/
```

---

## 発行済みリンク

| 流入元 | タグ値 | リンク | 貼る場所 | 発行日 |
| --- | --- | --- | --- | --- |
| ホワイトペーパー | `whitepaper` | `…/line/?ref=whitepaper` | 資料PDF内のCTA、ダウンロード完了画面 | 2026-09-26 |
| X記事 | `article_20260819` | `…/line/?ref=article_20260819` | 記事末尾のCTA | 2026-08-19 |
| X宣伝ポスト | `post_20260819` | `…/line/?ref=post_20260819` | 記事公開時の告知ポスト | 2026-08-19 |
| Xスレッド | `thread_20260819` | `…/line/?ref=thread_20260819` | スレッド最終ツイート | 2026-08-19 |
| Xプロフィール固定 | `profile` | `…/line/?ref=profile` | プロフィールのリンク欄 | 2026-08-19 |

### ホワイトペーパー用リンク（今回発行）

```
https://dakuiyongpu2-wq.github.io/x-analytics/line/?ref=whitepaper
```

資料を複数出す場合は末尾を分けて、この表に追記する。

```
…/line/?ref=whitepaper_x_kpi      ← 「X運用KPI設計」版
…/line/?ref=whitepaper_sheet      ← 「分析シート解説」版
```

---

## 命名ルール

- 使える文字は **英数字と `-` `_` `.`** のみ、**32文字以内**。
  それ以外の文字はページ側で `_` に置換されるので、日本語のタグ値は使わない。
- 形式は `<媒体・資料の種類>` か `<種類>_<識別子>`。
  継続的に使う導線（プロフィール、ホワイトペーパー）は日付を付けない。
  単発の投稿・記事は `_YYYYMMDD` を付けて使い回さない。
- 同じ流入元に2つ以上のタグを割り当てない（集計が割れる）。

## タグの自動付与

`?ref=` を付け忘れたリンクでも、リファラから自動でタグが付く。

| 流入元 | 自動で付くタグ |
| --- | --- |
| x.com / twitter.com / t.co | `x` |
| instagram.com | `instagram` |
| tiktok.com | `tiktok` |
| youtube.com / youtu.be | `youtube` |
| note.com | `note` |
| Google / Bing / Yahoo | `search` |
| その他のサイト | `web_<ホスト名>` |
| リファラなし（直打ち・PDF・QR経由など） | `direct` |

**PDFやQRコードからの流入はリファラが取れず `direct` に落ちる。**
ホワイトペーパーのように資料内に貼るリンクは、必ず `?ref=whitepaper` を明示すること。

優先順位は `?ref=` 指定 → 同一セッションで確定済みのタグ → リファラ判定 → `direct`。

## 集計

`line/index.html` の `CONFIG.TRACK_URL` にGASのWebアプリURLを設定すると、
CTAクリック時に以下のJSONが送られる。未設定なら送信しない。

```json
{ "type": "line_cta_click", "ref": "whitepaper", "place": "main", "ua": "…", "at": "2026-09-26T…Z" }
```

`place` は `main`（本文内CTA）か `sticky`（追従バー）。
