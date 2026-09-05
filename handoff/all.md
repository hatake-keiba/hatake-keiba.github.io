# デザイン相談依頼書 — HATAKE / TRACK CONDITION ページ

このファイルをそのまま ChatGPT に貼って、下の「相談したいこと」に答えてもらってください。
添付するファイル: `current_design.html`（現在のデザイン案）、`track_options.html`（トラック図5案）、`live_current.html`（現在公開中の旧デザイン、参考）

---

## 1. これは何のページか

日本の中央競馬（JRA）のデータサイト「HATAKE」の1ページ、**TRACK CONDITION**。
その日に開催している競馬場（通常2〜3場）の **馬場状態・風・天気** を毎時更新で見せる。

読者は **競馬の予想をする人**。スマホで、レース直前にざっと見る。
「今日の芝は硬いのか」「風は直線でどっちに吹くのか」「先週より乾いたのか」を数秒で掴みたい。

姉妹ページに RACE TREND（各レースの出馬表と傾向）があり、下部ナビで行き来する。

**アクセス実績**: このページ 15,208 UU / 35,187 PV。RACE TREND（3,323 UU）の4倍以上で、サイトの集客の主力。

---

## 2. 表示している情報（すべて実データ。省略・改変は不可）

会場ごとに以下を出す。以下は 2026/09/05 札幌の実際の値。

| 項目 | 値 | 説明 |
|---|---|---|
| 会場 | 札幌（他に中山・阪神が同日開催） | 会場切替で表示を変える |
| 直線の向き | 北向き | 会場ごとに固定。方位で持っている |
| 回り | 右回り | 会場ごとに固定 |
| 風 | 北西 7m/s | アメダス実測。10分ごとに変わる |
| 風の効き | 横風・内へ5.0m | 直線の向きと風向きから計算した成分 |
| 芝 馬場状態 | 稍重 | 良／稍重／重／不良 |
| 芝 コース | Cコース | A/B/C（内柵の位置） |
| 芝丈 | 10-14cm | |
| 芝の状態（文章） | 「今週からCコースを使用する、コース全周の内柵沿いにカバーしきれない傷みがあり」 | JRA公式のコメント。長い |
| クッション値 | 7.6（前回 7.9 ↓） | 芝の硬さ。7〜12くらいの範囲 |
| 芝 含水率 | 13.9%（前回 13.5% ↑） | 5〜25%くらい |
| 芝の馬場傾向 | ロング「高速化 ↑」／ショート「時計かかる（+1.1秒）」 | 近年トレンドと直前開催の比較。小さな折れ線グラフ付き |
| 想定される傾向 | 脚質「やや差し・追込有利」／枠順「やや外枠有利」＋根拠3行 | 過去データとの照合結果。◎○△で強さを示す |
| ダート 馬場状態 | 重 | |
| ダート 含水率 | 14.7%（前回 8.3% ↑） | |
| ダートの傾向 | 脚質「フラット」／枠順「はっきり内枠有利」＋根拠3行 | |
| この先12時間の天気 | 1時間ごと。天気アイコン・気温・降水量・湿度 | 気象庁モデル |
| 週間情報 | 7日分。日付・天気・雨量・散水の有無 | JRA公式 |
| 起伏プロファイル | 芝・ダートそれぞれ。高低差と直線長＋断面図 | |
| 最近の作業 | 「30日芝の生育管理のため散水を実施」など | JRA公式の文章 |

**重要**: クッション値・含水率などの数値パラメータ表示は必須。減らせない。

---

## 3. デザインの制約（守ってほしいこと）

- **絵文字は使わない**。アイコンは inline SVG（currentColor で塗る）
- **色を増やさない**。現在は深ティール #033236 / 白 / #F6F6F6 / ミント #dfeee6 の4色に絞っている
- **スマホ優先**（375px幅で破綻しないこと）。横スクロールを起こさない
- 外部CSS/JSライブラリは使わず、1つのHTMLで完結させる
- Google Fonts は使用可
- 静的HTML。Pythonが毎時生成する（動的な要素はCSSアニメーションのみ可）

---

## 4. これまでの経緯（同じ失敗を繰り返さないために）

依頼者からのフィードバック履歴:

1. 「文字が小さい・字間が狭い・カードに対して文字や図がギリギリ」→ 余白と文字サイズを大きくした
2. 「色を使いすぎ」→ 4色に絞った
3. 「AIっぽくない、洗練された、現代的なものがいい」
4. 参考UIとして、ミント〜白のグラデ背景・角丸の白カード・深ティールのアクセント・丸いアイコンバッジ・浮いた下部ナビ、という健康アプリ風のデザインが提示され、**この方向で確定**
5. **トラック図（コース図＋風）が何度もリテイク**。現在も未確定 ← 最大の課題

### トラック図で何がダメだったか（依頼者の指摘と自己分析）

- トラックが小さく、パネルの6割が空白
- 風の矢印がトラックを斜めに横切って「取り消し線」に見える
- 内柵の線が二重輪郭に見えてノイズ
- ラベル（直線・向正面）が不揃いで図から浮いている
- **いちばん大事な「直線で横風、内へ押される」が図に描かれていない**（文章で補っているだけ）
- 「小さな挿絵」であって、情報を伝える図になっていない

### トラック図への具体的な要望

- 向正面と直線に **進行方向を示す矢羽根（>>> のようなもの）** を置く。**静止でよい**
- 風は **3本程度の矢印** で、**動く**（CSSアニメーション）
- 風の矢印は **トラックに被ってよい**

---

## 5. 相談したいこと

### (A) トラック図（最優先）

`track_options.html` に5案ある。どれも決定打に欠ける。

1. 風の場（短い矢印を全面に敷く）
2. 直線クローズアップ（風を向かい成分・横成分に分解して数値表示）
3. 風配図オーバーレイ（方位環＋セクター塗り）
4. 帯そのものに描く（帯の内側へ小さな押し矢印）
5. 図＋数値タイル（左に線画、右に3タイル）

**質問**:
- この5案をどう評価するか。どれを軸にすべきか
- あるいは**まったく別のアプローチ**があるか
- 「コースの形」「走る向き」「風向き」「風が直線に与える影響」の4つを、スマホの狭い面積で同時に伝える最適解は何か
- 会場ごとに直線の向きが違う（北向き・北北東向き・北西向きなど）。**方位をどう扱うべきか**（図を回転させる？方位マークだけ置く？そもそも方位を捨てて「直線」「向正面」の相対関係だけにする？）

### (B) ページ全体

`current_design.html` を見て:
- 情報の並び順（風→芝→傾向→ダート→天気→週間）は適切か
- カードの分け方は適切か。統合・分割すべき箇所はあるか
- セグメント式ゲージ（塗り＝現在値、輪郭1コマ＝前回値）は「前回との比較」が伝わるか。もっと良い見せ方は
- 「芝の状態」の長い文章、「最近の作業」の文章をどう扱うべきか
- 3会場をどう見せるか（現在は切替ピル。全部並べる／アコーディオン等の選択肢もある）

### (C) 一段上へ

- このページを「よくできたデータサイト」から「思わず毎週見に来るページ」にするには何が足りないか
- 競馬の予想者が本当に知りたいことに対して、情報設計として抜けているものはあるか

---

## 6. 回答してほしい形式

- 具体的なHTML/CSSコードで示してほしい（そのまま試せるように）
- 「なぜそうするか」を短くでいいので添えてほしい
- 制約（3節）は必ず守ってほしい
- 日本語で

---

## 参考: 現在公開中のページ

https://hatake-keiba.github.io/condition.html

`live_current.html` が現在公開中の旧デザイン（緑ベース、これから置き換える対象）。
`current_design.html` が新デザイン案（この方向で確定、細部を詰めている段階）。


---

# 添付1: 現在のデザイン案 (current_design.html)

```html
<title>TRACK CONDITION ソフトUI案</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@500;600;700;800&family=Zen+Kaku+Gothic+New:wght@500;700;900&display=swap">
<style>
:root{--deep:#033236;--deep2:#0b4a4d;--mint:#dfeee6;--mint2:#cfe5da;--sage:#c9dfd3;--paper:#f6f8f6;--white:#fff;--ink:#0f1f1c;--mute:#6f847b;--faint:#a3b3ab;--line:#e6eee9}
*{box-sizing:border-box}
body{margin:0;background:linear-gradient(180deg,#cfe0d6 0%,#e6efe9 38%,#f4f7f5 100%);min-height:100vh;color:var(--ink);font-family:"Zen Kaku Gothic New","Plus Jakarta Sans","Hiragino Kaku Gothic ProN","Yu Gothic",sans-serif;font-size:14px;line-height:1.7;letter-spacing:.01em;padding:0 0 110px}
.en{font-family:"Plus Jakarta Sans","Zen Kaku Gothic New",sans-serif}
.num{font-family:"Plus Jakarta Sans",sans-serif;font-variant-numeric:tabular-nums}
.app{max-width:420px;margin:0 auto;padding:22px 18px 0}
svg.i{width:1em;height:1em;fill:none;stroke:currentColor;stroke-width:2;stroke-linecap:round;stroke-linejoin:round;vertical-align:-.15em}
/* マストヘッド: 大きく HATAKE */
.mast{text-align:center;margin:8px 0 18px}
.mast .wm{font-size:clamp(60px,17.5vw,74px);font-weight:800;letter-spacing:.12em;color:var(--deep);line-height:1;padding-left:.12em;white-space:nowrap}
.mast .row{display:flex;align-items:center;justify-content:center;gap:10px;margin-top:12px;flex-wrap:wrap}
.mast .pg{font-size:13px;font-weight:800;letter-spacing:.24em;color:var(--deep)}
.mast .dt{font-size:12.5px;font-weight:700;color:var(--mute)}
.lede{font-size:13px;color:var(--mute);margin:0 0 18px;text-align:center}
/* 会場ピル(参考の日付ピル) */
.venues{display:flex;gap:10px;margin-bottom:18px}
.vp{flex:1;background:#fff;border-radius:16px;padding:12px 6px;text-align:center;box-shadow:0 2px 10px rgba(3,50,54,.05)}
.vp b{display:block;font-size:16px;font-weight:900}
.vp span{display:block;font-size:10.5px;color:var(--mute);margin-top:2px}
.vp.on{background:var(--deep);color:#fff}.vp.on span{color:#9fc7c2}
.vp.dim b{color:var(--faint)}
/* カード共通 */
.card{background:#fff;border-radius:22px;padding:18px 18px 16px;box-shadow:0 4px 18px rgba(3,50,54,.06);margin-bottom:14px}
.ch{display:flex;align-items:center;gap:12px;margin-bottom:12px}
.ch .ic{width:40px;height:40px;border-radius:50%;background:var(--mint);display:flex;align-items:center;justify-content:center;color:var(--deep);flex:none}
.ch .ic svg{width:19px;height:19px}
.ch .tt{font-size:16px;font-weight:900;line-height:1.2}
.ch .ss{font-size:11.5px;color:var(--mute);margin-top:2px}
.ch .rt{margin-left:auto;font-size:11.5px;color:var(--mute);background:var(--paper);border-radius:14px;padding:5px 11px;white-space:nowrap}
.ch .rt b{color:var(--ink)}
/* トラック+風 */
.trackwrap{background:var(--mint);border-radius:18px;padding:12px 12px 6px;position:relative}
.trackwrap svg.trk{width:100%;height:auto;display:block}
.windrow{display:flex;align-items:center;gap:12px;margin-top:12px}
.windrow .big{font-size:32px;font-weight:800;letter-spacing:-.02em;line-height:1}
.windrow .big small{font-size:12px;font-weight:700;color:var(--mute);margin-left:4px;letter-spacing:0}
.windrow .desc{font-size:12.5px;color:var(--mute);line-height:1.5}
.windrow .desc b{color:var(--ink)}
.tag{display:inline-block;font-size:11px;font-weight:800;color:var(--deep);background:var(--mint);border-radius:12px;padding:3px 10px;margin-left:auto;white-space:nowrap}
/* ステータス行 */
.stat{display:flex;gap:10px;margin:2px 0 14px}
.stat div{flex:1;background:var(--paper);border-radius:14px;padding:10px 12px}
.stat .k{font-size:10.5px;color:var(--mute)}
.stat .v{font-size:16px;font-weight:900;margin-top:2px;letter-spacing:-.01em}
/* セグメント・ゲージ(参考の水分バー) */
.gg{margin-bottom:16px}.gg:last-child{margin-bottom:4px}
.gg .top{display:flex;align-items:baseline;justify-content:space-between;margin-bottom:8px}
.gg .k{font-size:12.5px;font-weight:700;color:var(--mute)}
.gg .v{font-size:24px;font-weight:800;letter-spacing:-.02em;line-height:1}
.gg .v small{font-size:12px;font-weight:700;color:var(--mute);margin-left:6px;letter-spacing:0}
.seg{display:flex;gap:3px;height:22px;padding:4px;background:var(--mint);border-radius:9px;position:relative}
.seg i{flex:1;border-radius:3px;background:rgba(255,255,255,.9)}
.seg i.f{background:var(--deep)}
.seg i.p{box-shadow:inset 0 0 0 2px var(--deep2);background:rgba(255,255,255,.9)}
.gg .ends{display:flex;justify-content:space-between;font-size:10.5px;color:var(--faint);margin-top:5px}
.note{font-size:12.5px;color:var(--mute);line-height:1.7;margin:0 0 12px}
.note b{color:var(--ink)}
/* 傾向 */
.chips{display:flex;flex-wrap:wrap;gap:8px;margin:2px 0 10px}
.chip{font-size:13px;font-weight:800;color:var(--deep);background:var(--mint);border-radius:14px;padding:7px 13px}
.chip.on{background:var(--deep);color:#fff}
.chip small{font-weight:700;opacity:.7;margin-right:4px}
.list{margin:0;padding:0;list-style:none}
.list li{font-size:13px;color:var(--mute);padding:6px 0;border-top:1px solid var(--line);display:flex;justify-content:space-between;gap:10px}
.list li:first-child{border-top:0}
.list li b{color:var(--ink);font-weight:700}
.trend{display:flex;gap:10px}
.trend div{flex:1;background:var(--paper);border-radius:14px;padding:10px 12px}
.trend .k{font-size:10.5px;color:var(--mute)}
.trend .v{font-size:14.5px;font-weight:900;margin-top:2px}
.trend .d{font-size:10.5px;color:var(--faint);margin-top:2px;line-height:1.4}
/* 天気(横スクロールのピル) */
.hours{display:flex;gap:8px;overflow-x:auto;padding:2px 0 6px;scrollbar-width:none}
.hours::-webkit-scrollbar{display:none}
.hr{flex:none;width:60px;background:var(--paper);border-radius:16px;padding:10px 6px;text-align:center}
.hr .t{font-size:11px;color:var(--mute)}
.hr svg{width:20px;height:20px;color:var(--deep);margin:6px 0 4px;display:block;margin-left:auto;margin-right:auto}
.hr b{display:block;font-size:15px;font-weight:800}
.hr .p{font-size:10px;color:var(--faint);margin-top:2px}
.hr.now{background:var(--deep);color:#fff}.hr.now .t,.hr.now .p{color:#9fc7c2}.hr.now svg{color:#fff}
.week{display:grid;grid-template-columns:repeat(7,1fr);gap:4px;margin-top:4px}
.wd{text-align:center;font-size:10.5px;color:var(--mute);padding:8px 0;border-radius:12px}
.wd b{display:block;font-size:12.5px;font-weight:800;color:var(--ink)}
.wd svg{width:16px;height:16px;color:var(--deep);display:block;margin:4px auto}
.wd .r{font-size:10px;color:var(--deep);font-weight:800}
.wd.today{background:var(--mint)}
/* 下部ナビ */
.nav{position:fixed;left:0;right:0;bottom:16px;display:flex;justify-content:center;pointer-events:none}
.nav div{pointer-events:auto;display:flex;gap:4px;background:var(--deep);border-radius:40px;padding:6px;box-shadow:0 10px 30px rgba(3,50,54,.28)}
.nav a{display:flex;align-items:center;gap:7px;color:#cfe5da;text-decoration:none;font-size:12.5px;font-weight:800;padding:11px 20px;border-radius:32px}
.nav a.on{background:#fff;color:var(--deep)}
.nav a svg{width:16px;height:16px}
.foot{font-size:11px;color:var(--faint);text-align:center;margin-top:18px}


/* 風の場: 全面の短い矢印が風下へ静かに流れる。トラックの上は白帯が上に乗るので邪魔しない */
.wf{fill:none;stroke:#5f8f83;stroke-width:1.6;stroke-linecap:round;stroke-linejoin:round;opacity:.45}
.wfield{animation:wdrift 2.8s linear infinite}
@keyframes wdrift{from{transform:translate(-6px,-6px);opacity:.9}to{transform:translate(6px,6px);opacity:.9}}
.push{animation:pushpulse 2.4s ease-in-out infinite}
@keyframes pushpulse{0%,100%{transform:translateX(0)}50%{transform:translateX(4px)}}
@media(prefers-reduced-motion:reduce){.wfield,.push{animation:none}}
.trackwrap{padding:10px 10px 4px}
</style>
<svg width="0" height="0" style="position:absolute">
  <symbol id="sun" viewBox="0 0 24 24"><circle cx="12" cy="12" r="4"/><path d="M12 2v2M12 20v2M2 12h2M20 12h2M5 5l1.4 1.4M17.6 17.6L19 19M19 5l-1.4 1.4M6.4 17.6L5 19"/></symbol>
  <symbol id="cloud" viewBox="0 0 24 24"><path d="M7 18h10a4 4 0 0 0 .6-7.95 5 5 0 0 0-9.6-1.2A3.5 3.5 0 0 0 7 18z"/></symbol>
  <symbol id="rain" viewBox="0 0 24 24"><path d="M7 15h10a4 4 0 0 0 .6-7.95 5 5 0 0 0-9.6-1.2A3.5 3.5 0 0 0 7 15z"/><path d="M8 18l-1 3M12 18l-1 3M16 18l-1 3"/></symbol>
  <symbol id="wind" viewBox="0 0 24 24"><path d="M3 8h11a3 3 0 1 0-3-3"/><path d="M3 14h14a3 3 0 1 1-3 3"/></symbol>
  <symbol id="leaf" viewBox="0 0 24 24"><path d="M5 21c0-9 5-14 14-14 0 9-5 14-14 14z"/><path d="M5 21c4-5 8-8 12-10"/></symbol>
  <symbol id="grid" viewBox="0 0 24 24"><rect x="3" y="3" width="8" height="8" rx="2"/><rect x="13" y="3" width="8" height="8" rx="2"/><rect x="3" y="13" width="8" height="8" rx="2"/><rect x="13" y="13" width="8" height="8" rx="2"/></symbol>
  <symbol id="flag" viewBox="0 0 24 24"><path d="M5 21V4"/><path d="M5 4c3-1.5 6 1.5 9 0s5-1 5-1v9s-2 .5-5 2-6-1-9 0"/></symbol>
  <symbol id="target" viewBox="0 0 24 24"><circle cx="12" cy="12" r="9"/><circle cx="12" cy="12" r="4"/><path d="M12 3v3M12 18v3M3 12h3M18 12h3"/></symbol>
  <symbol id="list" viewBox="0 0 24 24"><path d="M8 6h13M8 12h13M8 18h13"/><circle cx="4" cy="6" r="1"/><circle cx="4" cy="12" r="1"/><circle cx="4" cy="18" r="1"/></symbol>
  <symbol id="track" viewBox="0 0 320 200">
    <!-- 風の場(面): 北西→南東へ向く短い矢印を全面に。風向きが変わればここが回る -->
    <g class="wfield"><path class="wf" d="M15.4 19.4L24.6 28.6M23.9 22.2L24.6 28.6L18.2 27.9"/><path class="wf" d="M55.4 19.4L64.6 28.6M63.9 22.2L64.6 28.6L58.2 27.9"/><path class="wf" d="M95.4 19.4L104.6 28.6M103.9 22.2L104.6 28.6L98.2 27.9"/><path class="wf" d="M135.4 19.4L144.6 28.6M143.9 22.2L144.6 28.6L138.2 27.9"/><path class="wf" d="M175.4 19.4L184.6 28.6M183.9 22.2L184.6 28.6L178.2 27.9"/><path class="wf" d="M215.4 19.4L224.6 28.6M223.9 22.2L224.6 28.6L218.2 27.9"/><path class="wf" d="M255.4 19.4L264.6 28.6M263.9 22.2L264.6 28.6L258.2 27.9"/><path class="wf" d="M295.4 19.4L304.6 28.6M303.9 22.2L304.6 28.6L298.2 27.9"/><path class="wf" d="M31.4 51.4L40.6 60.6M39.9 54.2L40.6 60.6L34.2 59.9"/><path class="wf" d="M71.4 51.4L80.6 60.6M79.9 54.2L80.6 60.6L74.2 59.9"/><path class="wf" d="M111.4 51.4L120.6 60.6M119.9 54.2L120.6 60.6L114.2 59.9"/><path class="wf" d="M151.4 51.4L160.6 60.6M159.9 54.2L160.6 60.6L154.2 59.9"/><path class="wf" d="M191.4 51.4L200.6 60.6M199.9 54.2L200.6 60.6L194.2 59.9"/><path class="wf" d="M231.4 51.4L240.6 60.6M239.9 54.2L240.6 60.6L234.2 59.9"/><path class="wf" d="M271.4 51.4L280.6 60.6M279.9 54.2L280.6 60.6L274.2 59.9"/><path class="wf" d="M311.4 51.4L320.6 60.6M319.9 54.2L320.6 60.6L314.2 59.9"/><path class="wf" d="M15.4 83.4L24.6 92.6M23.9 86.2L24.6 92.6L18.2 91.9"/><path class="wf" d="M55.4 83.4L64.6 92.6M63.9 86.2L64.6 92.6L58.2 91.9"/><path class="wf" d="M95.4 83.4L104.6 92.6M103.9 86.2L104.6 92.6L98.2 91.9"/><path class="wf" d="M135.4 83.4L144.6 92.6M143.9 86.2L144.6 92.6L138.2 91.9"/><path class="wf" d="M175.4 83.4L184.6 92.6M183.9 86.2L184.6 92.6L178.2 91.9"/><path class="wf" d="M215.4 83.4L224.6 92.6M223.9 86.2L224.6 92.6L218.2 91.9"/><path class="wf" d="M255.4 83.4L264.6 92.6M263.9 86.2L264.6 92.6L258.2 91.9"/><path class="wf" d="M295.4 83.4L304.6 92.6M303.9 86.2L304.6 92.6L298.2 91.9"/><path class="wf" d="M31.4 115.4L40.6 124.6M39.9 118.2L40.6 124.6L34.2 123.9"/><path class="wf" d="M71.4 115.4L80.6 124.6M79.9 118.2L80.6 124.6L74.2 123.9"/><path class="wf" d="M111.4 115.4L120.6 124.6M119.9 118.2L120.6 124.6L114.2 123.9"/><path class="wf" d="M151.4 115.4L160.6 124.6M159.9 118.2L160.6 124.6L154.2 123.9"/><path class="wf" d="M191.4 115.4L200.6 124.6M199.9 118.2L200.6 124.6L194.2 123.9"/><path class="wf" d="M231.4 115.4L240.6 124.6M239.9 118.2L240.6 124.6L234.2 123.9"/><path class="wf" d="M271.4 115.4L280.6 124.6M279.9 118.2L280.6 124.6L274.2 123.9"/><path class="wf" d="M311.4 115.4L320.6 124.6M319.9 118.2L320.6 124.6L314.2 123.9"/><path class="wf" d="M15.4 147.4L24.6 156.6M23.9 150.2L24.6 156.6L18.2 155.9"/><path class="wf" d="M55.4 147.4L64.6 156.6M63.9 150.2L64.6 156.6L58.2 155.9"/><path class="wf" d="M95.4 147.4L104.6 156.6M103.9 150.2L104.6 156.6L98.2 155.9"/><path class="wf" d="M135.4 147.4L144.6 156.6M143.9 150.2L144.6 156.6L138.2 155.9"/><path class="wf" d="M175.4 147.4L184.6 156.6M183.9 150.2L184.6 156.6L178.2 155.9"/><path class="wf" d="M215.4 147.4L224.6 156.6M223.9 150.2L224.6 156.6L218.2 155.9"/><path class="wf" d="M255.4 147.4L264.6 156.6M263.9 150.2L264.6 156.6L258.2 155.9"/><path class="wf" d="M295.4 147.4L304.6 156.6M303.9 150.2L304.6 156.6L298.2 155.9"/><path class="wf" d="M31.4 179.4L40.6 188.6M39.9 182.2L40.6 188.6L34.2 187.9"/><path class="wf" d="M71.4 179.4L80.6 188.6M79.9 182.2L80.6 188.6L74.2 187.9"/><path class="wf" d="M111.4 179.4L120.6 188.6M119.9 182.2L120.6 188.6L114.2 187.9"/><path class="wf" d="M151.4 179.4L160.6 188.6M159.9 182.2L160.6 188.6L154.2 187.9"/><path class="wf" d="M191.4 179.4L200.6 188.6M199.9 182.2L200.6 188.6L194.2 187.9"/><path class="wf" d="M231.4 179.4L240.6 188.6M239.9 182.2L240.6 188.6L234.2 187.9"/><path class="wf" d="M271.4 179.4L280.6 188.6M279.9 182.2L280.6 188.6L274.2 187.9"/><path class="wf" d="M311.4 179.4L320.6 188.6M319.9 182.2L320.6 188.6L314.2 187.9"/></g>
    <!-- トラック: 一本の帯(外周のみ・二重線なし)。右回り=直線は西側で北向き -->
    <rect x="110" y="20" width="100" height="160" rx="50" fill="none" stroke="#fff" stroke-width="20"/>
    <!-- 直線(西側): 深ティールの帯 + ゴール線 + 進行方向の矢羽根(静止) -->
    <path d="M110 134V66" stroke="#033236" stroke-width="20" fill="none"/>
    <path d="M100 66h20" stroke="#fff" stroke-width="2.5"/>
    <path d="M104 122l6-7 6 7M104 104l6-7 6 7M104 86l6-7 6 7" fill="none" stroke="#fff" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round"/>
    <!-- 向正面(東側): 白い帯の上に進行方向の矢羽根(静止) -->
    <path d="M204 78l6 7 6-7M204 96l6 7 6-7M204 114l6 7 6-7" fill="none" stroke="#033236" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round"/>
    <!-- ラベル: 帯に沿わせて同じ書式で -->
    <text x="110" y="54" font-size="10.5" fill="#033236" font-weight="800" text-anchor="middle">直線</text>
    <text x="110" y="196" font-size="9" fill="#6f847b" font-weight="700" text-anchor="middle">ゴール →北</text>
    <text x="210" y="54" font-size="10.5" fill="#6f847b" font-weight="800" text-anchor="middle">向正面</text>
    <!-- 直線に当たる風の効き: 内へ押される向きを1本だけ強調 -->
    <path class="push" d="M66 100H96" stroke="#033236" stroke-width="3.5" fill="none" stroke-linecap="round"/>
    <path class="push" d="M89 94l7 6-7 6" stroke="#033236" stroke-width="3.5" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
    <text x="80" y="118" font-size="10" fill="#033236" font-weight="800" text-anchor="middle">内へ 5.0m</text>
    <!-- 方位: 隅に小さく1つ -->
    <circle cx="292" cy="28" r="14" fill="#fff"/>
    <path d="M292 18l5 12-5-3-5 3z" fill="#033236"/>
    <text x="292" y="49" font-size="8" fill="#6f847b" font-weight="700" text-anchor="middle">N</text>
  </symbol>
</svg>

<div class="app">
  <div class="mast">
    <div class="wm en">HATAKE</div>
    <div class="row"><span class="pg en">TRACK CONDITION</span><span class="dt num">9/5（土）</span></div>
  </div>

  <div class="venues">
    <div class="vp on"><b>札幌</b><span>芝 稍重</span></div>
    <div class="vp dim"><b>中山</b><span>馬場は午後</span></div>
    <div class="vp dim"><b>阪神</b><span>馬場は午後</span></div>
  </div>

  <!-- 風 × トラック -->
  <div class="card">
    <div class="ch"><div class="ic"><svg class="i"><use href="#wind"/></svg></div><div><div class="tt">札幌競馬場</div><div class="ss">直線 北向き ・ 右回り ・ 13:00 現在</div></div><a class="rt" href="#">JRA公式 <b>写真</b></a></div>
    <div class="trackwrap"><svg class="trk" viewBox="0 0 320 200"><use href="#track"/></svg></div>
    <div class="windrow"><div class="big num">7<small>m/s</small></div><div class="desc"><b>北西の風</b><br>直線に対して横風。内へ5.0m押される</div><span class="tag">横風</span></div>
  </div>

  <!-- 芝 -->
  <div class="card">
    <div class="ch"><div class="ic"><svg class="i"><use href="#leaf"/></svg></div><div><div class="tt">芝</div><div class="ss">Cコース ・ 芝丈 10–14cm</div></div><span class="rt">馬場 <b>稍重</b></span></div>
    <p class="note"><b>芝の状態</b> 今週からCコースを使用する、コース全周の内柵沿いにカバーしきれない傷みがあり</p>
    <div class="gg"><div class="top"><span class="k">クッション値</span><span class="v num">7.6<small>前 7.9 ↓</small></span></div>
      <div class="seg"><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="p"></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i></div>
      <div class="ends"><span>軟らか</span><span>硬め</span></div></div>
    <div class="gg"><div class="top"><span class="k">芝 含水率</span><span class="v num">13.9<small>% ・ 前 13.5 ↑</small></span></div>
      <div class="seg"><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i></div>
      <div class="ends"><span>乾燥</span><span>湿潤</span></div></div>
    <div class="trend" style="margin-top:14px"><div><div class="k">ロング ・ 近年の傾向</div><div class="v">高速化 ↑</div></div><div><div class="k">ショート ・ 直前開催</div><div class="v">時計かかる</div><div class="d">開催平均より +1.1秒</div></div></div>
  </div>

  <!-- 傾向(芝) -->
  <div class="card">
    <div class="ch"><div class="ic"><svg class="i"><use href="#target"/></svg></div><div><div class="tt">今日の傾向（芝）</div><div class="ss">クッション値・馬場状態・風の3点を過去データと照合</div></div></div>
    <div class="chips"><span class="chip on"><small>脚質</small>やや差し・追込有利</span><span class="chip"><small>枠</small>やや外枠有利</span></div>
    <ul class="list">
      <li><span>横風が内へ 5.0m</span><b>内枠やや有利 ○</b></li>
      <li><span>道悪</span><b>差し有利 △</b></li>
      <li><span>道悪</span><b>外枠有利 ○</b></li>
    </ul>
  </div>

  <!-- ダート -->
  <div class="card">
    <div class="ch"><div class="ic"><svg class="i"><use href="#grid"/></svg></div><div><div class="tt">ダート</div><div class="ss">前 稍重</div></div><span class="rt">馬場 <b>重</b></span></div>
    <div class="gg"><div class="top"><span class="k">ダート 含水率</span><span class="v num">14.7<small>% ・ 前 8.3 ↑</small></span></div>
      <div class="seg"><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i class="f"></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i></div>
      <div class="ends"><span>乾燥</span><span>湿潤</span></div></div>
    <div class="chips" style="margin-top:12px"><span class="chip"><small>脚質</small>フラット</span><span class="chip on"><small>枠</small>はっきり内枠有利</span></div>
    <ul class="list"><li><span>横風が内へ 5.0m</span><b>内枠やや有利 △</b></li><li><span>道悪</span><b>差し有利 △</b></li><li><span>道悪</span><b>内枠有利 ○</b></li></ul>
  </div>

  <!-- 天気 -->
  <div class="card">
    <div class="ch"><div class="ic"><svg class="i"><use href="#sun"/></svg></div><div><div class="tt">この先12時間</div><div class="ss">1時間ごと ・ 気象庁モデル ・ 札幌競馬場地点</div></div></div>
    <div class="hours">
      <div class="hr now"><div class="t">13時</div><svg class="i"><use href="#sun"/></svg><b class="num">20°</b><div class="p">66%</div></div>
      <div class="hr"><div class="t">14時</div><svg class="i"><use href="#sun"/></svg><b class="num">20°</b><div class="p">67%</div></div>
      <div class="hr"><div class="t">15時</div><svg class="i"><use href="#sun"/></svg><b class="num">19°</b><div class="p">70%</div></div>
      <div class="hr"><div class="t">16時</div><svg class="i"><use href="#cloud"/></svg><b class="num">19°</b><div class="p">70%</div></div>
      <div class="hr"><div class="t">17時</div><svg class="i"><use href="#cloud"/></svg><b class="num">19°</b><div class="p">69%</div></div>
      <div class="hr"><div class="t">18時</div><svg class="i"><use href="#cloud"/></svg><b class="num">18°</b><div class="p">66%</div></div>
      <div class="hr"><div class="t">19時</div><svg class="i"><use href="#cloud"/></svg><b class="num">17°</b><div class="p">69%</div></div>
      <div class="hr"><div class="t">20時</div><svg class="i"><use href="#cloud"/></svg><b class="num">16°</b><div class="p">72%</div></div>
    </div>
  </div>

  <!-- 週間 -->
  <div class="card">
    <div class="ch"><div class="ic"><svg class="i"><use href="#list"/></svg></div><div><div class="tt">週間情報</div><div class="ss">JRA公式 ・ 雨量と散水</div></div></div>
    <div class="week">
      <div class="wd">7/28<svg class="i"><use href="#rain"/></svg><b class="num">5.0</b><span class="r">mm</span></div>
      <div class="wd">7/29<svg class="i"><use href="#cloud"/></svg><b class="num">0</b></div>
      <div class="wd">7/30<svg class="i"><use href="#sun"/></svg><b class="num">0</b><span class="r">散水</span></div>
      <div class="wd">7/31<svg class="i"><use href="#sun"/></svg><b class="num">0</b></div>
      <div class="wd">8/1<svg class="i"><use href="#cloud"/></svg><b class="num">2.0</b><span class="r">mm</span></div>
      <div class="wd">8/2<svg class="i"><use href="#rain"/></svg><b class="num">5.0</b><span class="r">mm</span></div>
      <div class="wd today">今日<svg class="i"><use href="#sun"/></svg><b class="num">0</b></div>
    </div>
    <p class="note" style="margin:12px 0 0">最近の作業 ・ 30日芝の生育管理のため散水を実施 ・ 31日、2日から3日クッション砂の砂厚を調整（9.0センチメートル）</p>
  </div>

  <div class="foot">騎手・調教師の集計期間 〜2026/03/29　風はアメダス実測</div>
</div>

<div class="nav"><div>
  <a class="on" href="#"><svg class="i"><use href="#leaf"/></svg>Condition</a>
  <a href="#"><svg class="i"><use href="#flag"/></svg>Race</a>
</div></div>

```

---

# 添付2: トラック図5案 (track_options.html)

```html
<title>トラック図 再設計5案</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@600;700;800&family=Zen+Kaku+Gothic+New:wght@500;700;900&display=swap">
<style>
:root{--deep:#033236;--mint:#dfeee6;--mint2:#cfe5da;--paper:#f6f8f6;--ink:#0f1f1c;--mute:#6f847b;--faint:#a3b3ab;--w:#5f8f83}
*{box-sizing:border-box}
body{margin:0;background:linear-gradient(180deg,#cfe0d6,#eef3ef);color:var(--ink);font-family:"Zen Kaku Gothic New","Plus Jakarta Sans",sans-serif;font-size:14px;line-height:1.6;padding:20px 0 40px}
.col{width:375px;max-width:100%;margin:0 auto 26px;padding:0 14px}
.lbl{font-size:11px;font-weight:800;letter-spacing:.1em;color:#3f5a52;margin:0 4px 8px}
.lbl b{font-size:12px;margin-right:8px;color:var(--deep)}
.lbl i{font-style:normal;display:block;font-weight:500;color:#6f847b;letter-spacing:0;font-size:11px;line-height:1.55}
.num{font-family:"Plus Jakarta Sans",sans-serif;font-variant-numeric:tabular-nums}
.card{background:#fff;border-radius:22px;padding:18px 18px 16px;box-shadow:0 4px 18px rgba(3,50,54,.06)}
.ch{display:flex;align-items:center;gap:12px;margin-bottom:12px}
.ch .ic{width:40px;height:40px;border-radius:50%;background:var(--mint);display:flex;align-items:center;justify-content:center;color:var(--deep)}
.ch .ic svg{width:19px;height:19px;fill:none;stroke:currentColor;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
.ch .tt{font-size:16px;font-weight:900;line-height:1.2}.ch .ss{font-size:11.5px;color:var(--mute);margin-top:2px}
.panel{background:var(--mint);border-radius:18px;padding:10px 10px 4px;overflow:hidden}
.panel svg{width:100%;height:auto;display:block}
.windrow{display:flex;align-items:center;gap:12px;margin-top:12px}
.windrow .big{font-size:32px;font-weight:800;letter-spacing:-.02em;line-height:1}
.windrow .big small{font-size:12px;font-weight:700;color:var(--mute);margin-left:4px;letter-spacing:0}
.windrow .desc{font-size:12.5px;color:var(--mute);line-height:1.5}.windrow .desc b{color:var(--ink)}
.tag{font-size:11px;font-weight:800;color:var(--deep);background:var(--mint);border-radius:12px;padding:3px 10px;margin-left:auto;white-space:nowrap}
text{font-family:"Zen Kaku Gothic New",sans-serif}
.t1{font-size:10.5px;font-weight:800;fill:var(--deep)}.t2{font-size:10.5px;font-weight:800;fill:var(--mute)}.t3{font-size:9px;font-weight:700;fill:var(--mute)}
.tn{font-family:"Plus Jakarta Sans",sans-serif;font-weight:800;fill:var(--deep)}
.band{fill:none;stroke:#fff;stroke-width:20}
.str{stroke:var(--deep);stroke-width:20;fill:none}
.chv{fill:none;stroke-width:2.6;stroke-linecap:round;stroke-linejoin:round}
.wf{fill:none;stroke:var(--w);stroke-width:1.6;stroke-linecap:round;stroke-linejoin:round;opacity:.45}
.wfield{animation:wdrift 2.8s linear infinite}
@keyframes wdrift{from{transform:translate(-6px,-6px)}to{transform:translate(6px,6px)}}
.push{animation:pp 2.4s ease-in-out infinite}
@keyframes pp{0%,100%{transform:translateX(0)}50%{transform:translateX(4px)}}
.bigw{animation:bw 3.2s ease-in-out infinite}
@keyframes bw{0%,100%{transform:translate(-5px,-5px);opacity:.9}50%{transform:translate(5px,5px);opacity:.6}}
.gust{fill:none;stroke:var(--w);stroke-width:2.4;stroke-linecap:round;stroke-linejoin:round;animation:gg 2.6s ease-in-out infinite}
.gust:nth-child(2){animation-delay:-.9s}.gust:nth-child(3){animation-delay:-1.8s}
@keyframes gg{0%{opacity:0;transform:translate(-12px,-12px)}25%{opacity:.7}70%{opacity:.7}100%{opacity:0;transform:translate(12px,12px)}}
@media(prefers-reduced-motion:reduce){*{animation:none!important}}
/* 5 tiles */
.split{display:grid;grid-template-columns:1fr 112px;gap:10px;align-items:stretch}
.tiles{display:flex;flex-direction:column;gap:8px}
.tile{background:#fff;border-radius:14px;padding:10px 12px;flex:1}
.tile .k{font-size:10px;color:var(--mute);font-weight:700}
.tile .v{font-size:18px;font-weight:800;letter-spacing:-.02em;line-height:1.1;margin-top:2px}
.tile .v small{font-size:11px;color:var(--mute);margin-left:3px;font-weight:700}
.tile.on{background:var(--deep);color:#fff}.tile.on .k{color:#9fc7c2}.tile.on .v small{color:#9fc7c2}
</style>
<svg width="0" height="0" style="position:absolute"><symbol id="wind" viewBox="0 0 24 24"><path d="M3 8h11a3 3 0 1 0-3-3"/><path d="M3 14h14a3 3 0 1 1-3 3"/></symbol></svg>

<!-- ============ 1 風の場 ============ -->
<div class="col"><div class="lbl"><b>1</b>風の場<i>短い矢印を全面に敷いて「風が吹いている面」にする。トラックはパネルいっぱい、直線に当たる押し方向だけ1本強調。方位は隅にN</i></div>
<div class="card"><div class="ch"><div class="ic"><svg viewBox="0 0 24 24"><use href="#wind"/></svg></div><div><div class="tt">札幌競馬場</div><div class="ss">直線 北向き ・ 右回り ・ 13:00 現在</div></div></div>
<div class="panel"><svg viewBox="0 0 320 200">
<g class="wfield">
<path class="wf" d="M15.4 19.4L24.6 28.6M25.2 22.5L24.6 28.6L18.6 28.0"/><path class="wf" d="M55.4 19.4L64.6 28.6M65.2 22.5L64.6 28.6L58.6 28.0"/><path class="wf" d="M95.4 19.4L104.6 28.6M105.2 22.5L104.6 28.6L98.6 28.0"/><path class="wf" d="M135.4 19.4L144.6 28.6M145.2 22.5L144.6 28.6L138.6 28.0"/><path class="wf" d="M175.4 19.4L184.6 28.6M185.2 22.5L184.6 28.6L178.6 28.0"/><path class="wf" d="M215.4 19.4L224.6 28.6M225.2 22.5L224.6 28.6L218.6 28.0"/><path class="wf" d="M255.4 19.4L264.6 28.6M265.2 22.5L264.6 28.6L258.6 28.0"/><path class="wf" d="M295.4 19.4L304.6 28.6M305.2 22.5L304.6 28.6L298.6 28.0"/>
<path class="wf" d="M31.4 51.4L40.6 60.6M41.2 54.5L40.6 60.6L34.6 60.0"/><path class="wf" d="M71.4 51.4L80.6 60.6M81.2 54.5L80.6 60.6L74.6 60.0"/><path class="wf" d="M111.4 51.4L120.6 60.6M121.2 54.5L120.6 60.6L114.6 60.0"/><path class="wf" d="M151.4 51.4L160.6 60.6M161.2 54.5L160.6 60.6L154.6 60.0"/><path class="wf" d="M191.4 51.4L200.6 60.6M201.2 54.5L200.6 60.6L194.6 60.0"/><path class="wf" d="M231.4 51.4L240.6 60.6M241.2 54.5L240.6 60.6L234.6 60.0"/><path class="wf" d="M271.4 51.4L280.6 60.6M281.2 54.5L280.6 60.6L274.6 60.0"/>
<path class="wf" d="M15.4 83.4L24.6 92.6M25.2 86.5L24.6 92.6L18.6 92.0"/><path class="wf" d="M55.4 83.4L64.6 92.6M65.2 86.5L64.6 92.6L58.6 92.0"/><path class="wf" d="M95.4 83.4L104.6 92.6M105.2 86.5L104.6 92.6L98.6 92.0"/><path class="wf" d="M135.4 83.4L144.6 92.6M145.2 86.5L144.6 92.6L138.6 92.0"/><path class="wf" d="M175.4 83.4L184.6 92.6M185.2 86.5L184.6 92.6L178.6 92.0"/><path class="wf" d="M215.4 83.4L224.6 92.6M225.2 86.5L224.6 92.6L218.6 92.0"/><path class="wf" d="M255.4 83.4L264.6 92.6M265.2 86.5L264.6 92.6L258.6 92.0"/><path class="wf" d="M295.4 83.4L304.6 92.6M305.2 86.5L304.6 92.6L298.6 92.0"/>
<path class="wf" d="M31.4 115.4L40.6 124.6M41.2 118.5L40.6 124.6L34.6 124.0"/><path class="wf" d="M71.4 115.4L80.6 124.6M81.2 118.5L80.6 124.6L74.6 124.0"/><path class="wf" d="M111.4 115.4L120.6 124.6M121.2 118.5L120.6 124.6L114.6 124.0"/><path class="wf" d="M151.4 115.4L160.6 124.6M161.2 118.5L160.6 124.6L154.6 124.0"/><path class="wf" d="M191.4 115.4L200.6 124.6M201.2 118.5L200.6 124.6L194.6 124.0"/><path class="wf" d="M231.4 115.4L240.6 124.6M241.2 118.5L240.6 124.6L234.6 124.0"/><path class="wf" d="M271.4 115.4L280.6 124.6M281.2 118.5L280.6 124.6L274.6 124.0"/>
<path class="wf" d="M15.4 147.4L24.6 156.6M25.2 150.5L24.6 156.6L18.6 156.0"/><path class="wf" d="M55.4 147.4L64.6 156.6M65.2 150.5L64.6 156.6L58.6 156.0"/><path class="wf" d="M95.4 147.4L104.6 156.6M105.2 150.5L104.6 156.6L98.6 156.0"/><path class="wf" d="M135.4 147.4L144.6 156.6M145.2 150.5L144.6 156.6L138.6 156.0"/><path class="wf" d="M175.4 147.4L184.6 156.6M185.2 150.5L184.6 156.6L178.6 156.0"/><path class="wf" d="M215.4 147.4L224.6 156.6M225.2 150.5L224.6 156.6L218.6 156.0"/><path class="wf" d="M255.4 147.4L264.6 156.6M265.2 150.5L264.6 156.6L258.6 156.0"/><path class="wf" d="M295.4 147.4L304.6 156.6M305.2 150.5L304.6 156.6L298.6 156.0"/>
<path class="wf" d="M31.4 179.4L40.6 188.6M41.2 182.5L40.6 188.6L34.6 188.0"/><path class="wf" d="M71.4 179.4L80.6 188.6M81.2 182.5L80.6 188.6L74.6 188.0"/><path class="wf" d="M111.4 179.4L120.6 188.6M121.2 182.5L120.6 188.6L114.6 188.0"/><path class="wf" d="M151.4 179.4L160.6 188.6M161.2 182.5L160.6 188.6L154.6 188.0"/><path class="wf" d="M191.4 179.4L200.6 188.6M201.2 182.5L200.6 188.6L194.6 188.0"/><path class="wf" d="M231.4 179.4L240.6 188.6M241.2 182.5L240.6 188.6L234.6 188.0"/><path class="wf" d="M271.4 179.4L280.6 188.6M281.2 182.5L280.6 188.6L274.6 188.0"/>
</g>
<rect class="band" x="110" y="20" width="100" height="160" rx="50"/>
<path class="str" d="M110 134V66"/><path d="M100 66h20" stroke="#fff" stroke-width="2.5"/>
<path class="chv" stroke="#fff" d="M104 122l6-7 6 7M104 104l6-7 6 7M104 86l6-7 6 7"/>
<path class="chv" stroke="#033236" d="M204 78l6 7 6-7M204 96l6 7 6-7M204 114l6 7 6-7"/>
<text class="t1" x="110" y="54" text-anchor="middle">直線</text><text class="t3" x="110" y="196" text-anchor="middle">ゴール →北</text><text class="t2" x="210" y="54" text-anchor="middle">向正面</text>
<g class="push"><path d="M64 100H96" stroke="#033236" stroke-width="3.5" stroke-linecap="round"/><path d="M89 94l7 6-7 6" fill="none" stroke="#033236" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/></g>
<text class="t1" x="80" y="118" text-anchor="middle">内へ 5.0m</text>
<circle cx="292" cy="28" r="14" fill="#fff"/><path d="M292 18l5 12-5-3-5 3z" fill="#033236"/><text class="t3" x="292" y="49" text-anchor="middle">N</text>
</svg></div>
<div class="windrow"><div class="big num">7<small>m/s</small></div><div class="desc"><b>北西の風</b><br>直線に対して横風。内へ5.0m押される</div><span class="tag">横風</span></div>
</div></div>

<!-- ============ 2 直線クローズアップ ============ -->
<div class="col"><div class="lbl"><b>2</b>直線クローズアップ<i>左に全体図を小さく置いて位置を示し、右で直線を拡大。風を「向かい／横」の2成分に分解して数値で描く。物理がそのまま見える</i></div>
<div class="card"><div class="ch"><div class="ic"><svg viewBox="0 0 24 24"><use href="#wind"/></svg></div><div><div class="tt">札幌競馬場</div><div class="ss">直線 北向き ・ 右回り ・ 13:00 現在</div></div></div>
<div class="panel"><svg viewBox="0 0 320 200">
<!-- 全体図(左) -->
<rect x="28" y="40" width="64" height="112" rx="32" fill="none" stroke="#fff" stroke-width="12"/>
<path d="M28 120V72" stroke="#033236" stroke-width="12"/>
<path class="chv" stroke="#fff" stroke-width="2" d="M24 110l4-5 4 5M24 96l4-5 4 5M24 82l4-5 4 5"/>
<path class="chv" stroke="#033236" stroke-width="2" d="M88 82l4 5 4-5M88 96l4 5 4-5M88 110l4 5 4-5"/>
<text class="t3" x="60" y="30" text-anchor="middle">全体 ・ 右回り</text>
<text class="t3" x="60" y="172" text-anchor="middle">N↑</text>
<path d="M40 96 L118 96" stroke="#a9c4b7" stroke-width="1.5" stroke-dasharray="3 3"/>
<!-- 直線拡大(右) -->
<rect x="150" y="16" width="44" height="170" rx="8" fill="#033236"/>
<path d="M142 26h60" stroke="#fff" stroke-width="2.5"/><text class="t3" x="205" y="30" fill="#fff">ゴール</text>
<path class="chv" stroke="#fff" stroke-width="3" d="M164 160l8-9 8 9M164 130l8-9 8 9M164 100l8-9 8 9M164 70l8-9 8 9"/>
<text class="t1" x="172" y="198" text-anchor="middle" fill="#033236">直線 北向き</text>
<!-- 風ベクトルの分解 -->
<g class="bigw"><path d="M232 46 L252 66" stroke="#5f8f83" stroke-width="3" stroke-linecap="round"/><path d="M252 66l-9-1M252 66l-1-9" fill="none" stroke="#5f8f83" stroke-width="3" stroke-linecap="round"/></g>
<text class="t2" x="262" y="44">北西 7m/s</text>
<!-- 横成分: 内(東)へ -->
<path d="M200 110H236" stroke="#033236" stroke-width="3.5" stroke-linecap="round"/><path d="M229 104l7 6-7 6" fill="none" stroke="#033236" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
<text class="t1" x="244" y="106">内へ</text><text class="tn" x="244" y="122" font-size="16">5.0<tspan class="t3" font-size="9"> m</tspan></text>
<!-- 縦成分: 向かい(南向きに押す) -->
<path d="M120 60V96" stroke="#5f8f83" stroke-width="3" stroke-linecap="round"/><path d="M114 89l6 7 6-7" fill="none" stroke="#5f8f83" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
<text class="t2" x="120" y="54" text-anchor="middle">向かい</text><text class="tn" x="120" y="116" text-anchor="middle" font-size="13">4.9<tspan class="t3" font-size="9"> m</tspan></text>
</svg></div>
<div class="windrow"><div class="big num">7<small>m/s</small></div><div class="desc"><b>北西の風</b><br>直線では横風成分が大きい。内へ5.0m</div><span class="tag">横風</span></div>
</div></div>

<!-- ============ 3 風配図 ============ -->
<div class="col"><div class="lbl"><b>3</b>風配図オーバーレイ<i>直線を中心に方位環を重ね、風が来る方向のセクターを塗る。矢印は1本だけ、直線に刺さる。「どこから来て、どこに当たるか」が最短で分かる</i></div>
<div class="card"><div class="ch"><div class="ic"><svg viewBox="0 0 24 24"><use href="#wind"/></svg></div><div><div class="tt">札幌競馬場</div><div class="ss">直線 北向き ・ 右回り ・ 13:00 現在</div></div></div>
<div class="panel"><svg viewBox="0 0 320 200">
<rect class="band" x="130" y="20" width="100" height="160" rx="50"/>
<path class="str" d="M130 134V66"/><path d="M120 66h20" stroke="#fff" stroke-width="2.5"/>
<path class="chv" stroke="#fff" d="M124 122l6-7 6 7M124 104l6-7 6 7M124 86l6-7 6 7"/>
<path class="chv" stroke="#033236" d="M224 78l6 7 6-7M224 96l6 7 6-7M224 114l6 7 6-7"/>
<text class="t1" x="130" y="54" text-anchor="middle">直線</text><text class="t2" x="230" y="54" text-anchor="middle">向正面</text>
<!-- 方位環(直線中央を中心) -->
<circle cx="130" cy="100" r="78" fill="none" stroke="#fff" stroke-width="1.5" stroke-dasharray="2 4" opacity=".9"/>
<text class="t3" x="130" y="18" text-anchor="middle">N</text><text class="t3" x="48" y="104" text-anchor="middle">W</text><text class="t3" x="130" y="188" text-anchor="middle">S</text>
<!-- 風の来るセクター(北西) -->
<path d="M130 100 L64 34 A78 78 0 0 1 95 27 Z" fill="#5f8f83" opacity=".22"/>
<!-- 1本の矢印: 北西から直線へ -->
<g class="bigw"><path d="M74 44 L118 88" stroke="#033236" stroke-width="4" stroke-linecap="round"/><path d="M118 88l-12-2M118 88l-2-12" fill="none" stroke="#033236" stroke-width="4" stroke-linecap="round"/></g>
<text class="t1" x="62" y="36" text-anchor="end">北西 7m/s</text>
<text class="t1" x="130" y="160" text-anchor="middle" fill="#033236">横風 ・ 内へ 5.0m</text>
</svg></div>
<div class="windrow"><div class="big num">7<small>m/s</small></div><div class="desc"><b>北西の風</b><br>直線に対して横風。内へ5.0m押される</div><span class="tag">横風</span></div>
</div></div>

<!-- ============ 4 帯に描く ============ -->
<div class="col"><div class="lbl"><b>4</b>帯そのものに描く<i>風の効きを直線の帯に直接描く。帯の内側へ小さな押し矢印を並べ、大きな風は薄い1本を背景に。要素が少なく、いちばん静か</i></div>
<div class="card"><div class="ch"><div class="ic"><svg viewBox="0 0 24 24"><use href="#wind"/></svg></div><div><div class="tt">札幌競馬場</div><div class="ss">直線 北向き ・ 右回り ・ 13:00 現在</div></div></div>
<div class="panel"><svg viewBox="0 0 320 200">
<g class="bigw" opacity=".35"><path d="M40 30 L160 150" stroke="#5f8f83" stroke-width="10" stroke-linecap="round"/><path d="M160 150l-26-4M160 150l-4-26" fill="none" stroke="#5f8f83" stroke-width="10" stroke-linecap="round" stroke-linejoin="round"/></g>
<rect class="band" x="110" y="20" width="100" height="160" rx="50"/>
<path class="str" d="M110 134V66"/><path d="M100 66h20" stroke="#fff" stroke-width="2.5"/>
<path class="chv" stroke="#fff" d="M104 122l6-7 6 7M104 104l6-7 6 7M104 86l6-7 6 7"/>
<path class="chv" stroke="#033236" d="M204 78l6 7 6-7M204 96l6 7 6-7M204 114l6 7 6-7"/>
<!-- 押し矢印: 帯の外側から内側へ、小さく3本 -->
<g class="push" style="color:#033236"><path d="M84 78h14M93 74l5 4-5 4M84 100h14M93 96l5 4-5 4M84 122h14M93 118l5 4-5 4" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/></g>
<text class="t1" x="110" y="54" text-anchor="middle">直線</text><text class="t2" x="210" y="54" text-anchor="middle">向正面</text>
<text class="t1" x="70" y="104" text-anchor="end">内へ 5.0m</text>
<text class="t2" x="40" y="26">北西 7m/s</text>
<circle cx="292" cy="28" r="14" fill="#fff"/><path d="M292 18l5 12-5-3-5 3z" fill="#033236"/>
</svg></div>
<div class="windrow"><div class="big num">7<small>m/s</small></div><div class="desc"><b>北西の風</b><br>直線に対して横風。内へ5.0m押される</div><span class="tag">横風</span></div>
</div></div>

<!-- ============ 5 図＋数値タイル ============ -->
<div class="col"><div class="lbl"><b>5</b>図＋数値タイル<i>左に線画のトラック、右に「風／直線での効き／内へ」の3タイル。図は位置関係だけ、数値はタイルで読む。カードの文法と一番なじむ</i></div>
<div class="card"><div class="ch"><div class="ic"><svg viewBox="0 0 24 24"><use href="#wind"/></svg></div><div><div class="tt">札幌競馬場</div><div class="ss">直線 北向き ・ 右回り ・ 13:00 現在</div></div></div>
<div class="panel"><div class="split">
<svg viewBox="0 0 200 200">
<rect class="band" x="50" y="20" width="100" height="160" rx="50"/>
<path class="str" d="M50 134V66"/><path d="M40 66h20" stroke="#fff" stroke-width="2.5"/>
<path class="chv" stroke="#fff" d="M44 122l6-7 6 7M44 104l6-7 6 7M44 86l6-7 6 7"/>
<path class="chv" stroke="#033236" d="M144 78l6 7 6-7M144 96l6 7 6-7M144 114l6 7 6-7"/>
<text class="t1" x="50" y="54" text-anchor="middle">直線</text><text class="t2" x="150" y="54" text-anchor="middle">向正面</text>
<g><path class="gust" d="M8 40 L44 76 M44 76l-10-2M44 76l-2-10"/><path class="gust" d="M22 24 L58 60 M58 60l-10-2M58 60l-2-10"/><path class="gust" d="M0 62 L36 98 M36 98l-10-2M36 98l-2-10"/></g>
<circle cx="180" cy="26" r="12" fill="#fff"/><path d="M180 17l4 11-4-2-4 2z" fill="#033236"/>
<text class="t3" x="50" y="196" text-anchor="middle">ゴール →北</text>
</svg>
<div class="tiles">
  <div class="tile"><div class="k">風</div><div class="v num">7<small>m/s 北西</small></div></div>
  <div class="tile on"><div class="k">直線での効き</div><div class="v">横風</div></div>
  <div class="tile"><div class="k">内へ押される</div><div class="v num">5.0<small>m</small></div></div>
</div>
</div></div>
<div class="windrow"><div class="big num">7<small>m/s</small></div><div class="desc"><b>北西の風</b><br>直線に対して横風。内へ5.0m押される</div><span class="tag">横風</span></div>
</div></div>

```
