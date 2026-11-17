---
name: "daily-subscription-four-pack"
description: "每日訂閱四套件：從 Notion 的 PressPlay、Substack、方格子訂閱筆記與信件報告，取指定日（預設前一天）資料各做一張 9:16 PNG 圖卡。使用者說「每日訂閱四套件」「訂閱四套件」時使用。"
---

# 每日訂閱四套件

產出：四張 9:16 直式資訊圖卡（PressPlay、Substack、方格子、信件報告），繁體中文、台灣用語。版面 1080×1920、3x 截圖；交付 3240×5760 PNG（另留 1080×1920 版）。**預設淺底深字**；使用者說「深色版」才用深色色票。資訊密度不要因為主題或解析度而減少。

目標日：使用者指定就用指定日（「今天的」＝當天）；排程在台北時間 19:00 之後執行時用當天；其餘未指定用「前一天」。在回覆註明。

## 1. 抓資料（Notion）

用 ToolSearch 載入 `mcp__Notion__notion-fetch`、`mcp__Notion__notion-query-data-sources`、`mcp__Notion__notion-search`。

資料來源（collection）：
- PressPlay 訂閱筆記：`collection://fc733342-99af-49f4-bc52-57862ec9fb8e`（欄位：標題、頻道、類型、一句話重點、date:發布時間:start）
- Substack 訂閱筆記：`collection://d9a690dc-5f2a-4654-9d02-0ac89e9a8d26`（標題、刊物、類型、一句話重點、date:發布時間:start）
- 方格子 訂閱筆記：`collection://ccdaae1f-9ab7-4b4d-bdba-60ad08e9f906`（標題、作者、類型、內容完整度、一句話重點、date:發布時間:start）
- 信件報告：`collection://6fcefa6a-1249-4176-8460-bc365df1573a`（名稱、date:日期:start、檔案數）

ID 失效時 fetch「每日報告」頁 `3a5c68eecc0f8076bbf0d082ec1279fd` 的「投資研究」區重新找四個資料庫。

**日期判斷（重要）**：發布時間存的是 UTC（Z）。台北日＝UTC+8；文章標題開頭的「M/D」就是台北日。SQL 先擈寬拿前後兩天，再以標題前綴篩選：

```sql
SELECT url,"標題","頻道","類型","date:發布時間:start" AS t,"一句話重點"
FROM "collection://…" WHERE "date:發布時間:start" >= '前一日' AND "date:發布時間:start" < '後一日' ORDER BY t DESC
```
（Substack 把頻道換成刊物；方格子換成作者、加內容完整度）。只收標題前綴＝目標日的文章。

- 從「一句話重點」挑出重點篇，再 fetch 全文取細節數字（篇數少的來源要讀全文）。
- 方格子常只有 1–3 篇：可放「延伸｜隔日凌晨」一則，必須在圖上與頁尾標明實際時間。
- 信件報告每天有兩頁：`…｜信件報告（內文與電子報）`（先讀）、`…｜信件報告（附件資料）`。附件頁常超過工具輸出上限而存成檔案，用 python 切字元範圍讀；至少讀完開頭「當日綜合分析」「投資建議」，並在回覆說明讀了哪些部分。
- 某來源當天 0 篇：告訴使用者，詢問是否改用最近一天或只做其他三張。

## 2. 編輯原則

- 主軸：綜合當天多數作者的共識，寫成兩行有判斷的大標（關鍵詞用 `<em>` 主色）＋一行副標。
- 每個觀點標出作者／刊物／券商名；作者觀點、預測、推測不寫成事實。
- 只用筆記裡有的數字，不自行補、不換算；「未查證／推測／僅部分內容」加 `<span class="tag">`。
- 各卡建議區塊：
  - PressPlay：盤面與籌碼 4 格 KPI → 作者觀點 2×2 卡（頂邊主色線、可加步驟標籤）→ 法人情報站報告摘要 → 資金面 KPI。
  - Substack：數據牆 3×2 KPI（每格標來源刊物）→ 通路調查／產業數據四格 → 多空對照兩欄（看多紅標、看空綠標）→ 其他觀點條列。
  - 方格子：主文 KPI＋重點條列＋對照框 → 觀察時間軸 → 第二篇（數據小長條）→ 三個提醒 → 延伸。
  - 信件報告：盤勢 4 格 KPI → 目標價變動表（個股／券商／舊→新／重點，4–6 列）→ 分歧最大三檔的目標價區間圖（最低～最高，其他券商用白點）→ 法說亮點 → 下跌本質與觀察方向條列。
- 頁尾：左「來源：Notion ○○（M/D 台北時間發布）｜數字引自筆記，未另查證」，右「非投資建議」。
- 信件報告的券商報告多在收盤前發出：當天盤面有大變動時，在盤勢區塊標題旁註明「未反映當日收盤」。
- 首次出現的台股個股附代號，如 貿聯-KY (3665)。

## 3. 設計系統

與「每日社群三套件」同一套：深色底、Noto Serif CJK TC 900 標題、Noto Sans CJK TC 內文、紅漲綠跌（`--up:#ff5d5d;--dn:#2fd48a`）。

**防毛邊規則**：內文、註記、表格一律 500 字重；用下方調亮過的 `--ink2`、`--mute`；文字最小 18px；交付前自行用 Lanczos 縮成 1080 寬。

深色色票（使用者指定「深色版」時）：
- PressPlay：`--bg:#0d0b14;--card:#17131f;--line:#2e2540;--ink:#f7f3fb;--ink2:#e8e3ee;--mute:#c0b9ca;--acc:#ff5c9a`，badge「P」，光暈 #7a1f4f 右上、#2a1f6b 左下
- Substack：`--bg:#100c08;--card:#1b1510;--line:#382b1f;--ink:#fbf6f1;--ink2:#ede6df;--mute:#c4b9af;--acc:#ff7a2a`，badge「S」，光暈 #8a3a0f 右上、#3b2a10 左下
- 方格子：`--bg:#08100c;--card:#101b15;--line:#213428;--ink:#f2f8f4;--ink2:#e0eae4;--mute:#b2c2b8;--acc:#4fd89a`，badge「方」，光暈 #115a3a 右上、#1a3b4a 左下
- 信件報告：`--bg:#090b18;--card:#121628;--line:#262c4a;--ink:#f3f4fb;--ink2:#e2e4f1;--mute:#b2b6ca;--acc:#8ea2ff`，badge「✉」，光暈 #2b3a9a 右上、#4a2a7a 左下

淺色色票（預設）：
- PressPlay：`--bg:#fbf5f8;--card:#ffffff;--line:#ead5df;--ink:#1d0f17;--ink2:#33222b;--mute:#6a5560;--acc:#c81e63`，光暈 #ffd0e2 右上、#ddd6fb 左下
- Substack：`--bg:#fcf7f2;--card:#ffffff;--line:#ecdccb;--ink:#20140a;--ink2:#36281c;--mute:#6b5a4b;--acc:#c2520a`，光暈 #ffd9b8 右上、#f3e3bf 左下
- 方格子：`--bg:#f3f9f5;--card:#ffffff;--line:#cfe3d6;--ink:#0b1c13;--ink2:#1f3227;--mute:#4d6356;--acc:#0b7d4c`，光暈 #bfeed6 右上、#c9e4ee 左下
- 信件報告：`--bg:#f4f5fc;--card:#ffffff;--line:#d5d9ee;--ink:#0d1024;--ink2:#22263d;--mute:#52576f;--acc:#3b4fd0`，光暈 #cdd5ff 右上、#e2d3f7 左下

**淺色主題的追加樣式**（接在 base.css 後或各頁 `<style>`）：

```css
body{--up:#d23131;--dn:#0f8a55}
.badge{color:#fff}
.card,.kpi{box-shadow:0 2px 10px rgba(20,30,60,.06)}
.tag{background:var(--bg)}
```
光暈改用下列淺色、`.glow{opacity:.55}`。深色主題沿用原本 `--up:#ff5d5d;--dn:#2fd48a`。

**交付前多檢查三件事**：①圖表標籤彼此或與圓點、長條重疊（放大局部看，重疊就併成一行或移位）；②KPI 標題或數值換行後只剩一個字（縮短標題或調小字級）；③來源寫在事件之前的內容要標出來（如「盤前」「報告發出時未反映當日收盤」），免得被讀成對事件的回應。

共用 `base.css`（寫入工作目錄 `cards/`）：

```css
*{margin:0;padding:0;box-sizing:border-box}
html,body{width:1080px;height:1920px;overflow:hidden}
body{font-family:"Noto Sans CJK TC",sans-serif;background:var(--bg);color:var(--ink);position:relative;text-rendering:geometricPrecision}
.wrap{position:absolute;inset:0;padding:56px 60px 44px;display:flex;flex-direction:column;gap:16px}
.glow{position:absolute;border-radius:50%;filter:blur(120px);opacity:.5;pointer-events:none}
header{display:flex;justify-content:space-between;align-items:flex-start;position:relative}
.brand{display:flex;align-items:center;gap:14px;font-size:24px;letter-spacing:.18em;color:var(--mute);font-weight:500}
.badge{width:54px;height:54px;border-radius:14px;background:var(--acc);display:grid;place-items:center;font-size:30px;font-weight:900;color:var(--bg);letter-spacing:0}
.date{text-align:right;font-family:"Noto Serif CJK TC";color:var(--mute);font-size:22px;line-height:1.3;font-weight:500}
.date b{display:block;font-size:52px;color:var(--ink);font-weight:900}
h1{font-family:"Noto Serif CJK TC";font-weight:900;font-size:68px;line-height:1.15;position:relative}
h1 em{font-style:normal;color:var(--acc)}
.sub{font-size:25px;color:var(--mute);line-height:1.5;font-weight:500}
.sec{font-size:22px;letter-spacing:.24em;color:var(--acc);font-weight:700;display:flex;align-items:center;gap:14px}
.sec::after{content:"";flex:1;height:1.5px;background:linear-gradient(90deg,var(--acc),transparent);opacity:.6}
.card{background:var(--card);border:1px solid var(--line);border-radius:22px;padding:22px 26px;position:relative}
.card p,td{font-weight:500}
.hl{color:var(--acc);font-weight:700}
.up{color:var(--up);font-weight:700}.dn{color:var(--dn);font-weight:700}
.tag{display:inline-block;font-size:19px;padding:2px 10px;border-radius:8px;border:1px solid var(--line);color:var(--mute);margin-left:8px;vertical-align:3px;font-weight:500}
footer{margin-top:auto;display:flex;justify-content:space-between;font-size:19px;color:var(--mute);border-top:1px solid var(--line);padding-top:14px;font-weight:500}
.grid{display:grid;gap:14px}
.kpi{background:var(--card);border:1px solid var(--line);border-radius:18px;padding:10px 18px}
.kpi .k{font-size:20px;color:var(--mute);font-weight:500}
.kpi .v{font-size:32px;font-weight:900;line-height:1.2}
.kpi .d{font-size:22px;font-weight:700}
ul.dot{list-style:none;display:flex;flex-direction:column;gap:9px}
ul.dot li{font-size:24px;line-height:1.5;color:var(--ink2);padding-left:24px;position:relative;font-weight:500}
ul.dot li::before{content:"";position:absolute;left:2px;top:15px;width:9px;height:9px;border-radius:2px;background:var(--acc);transform:rotate(45deg)}
ul.dot li b{color:var(--ink)}
table{width:100%;border-collapse:collapse;font-size:22px}
th{font-size:18px;color:var(--mute);font-weight:500;text-align:left;padding:0 6px 8px;border-bottom:1px solid var(--line)}
td{padding:9px 6px;border-bottom:1px solid var(--line);color:var(--ink2)}
td:first-child,td.n{color:var(--ink);font-weight:700}
tr:last-child td{border-bottom:none}
```

可加元件：
- 觀點卡：2 欄 grid，`border-top:3px solid var(--acc)`，作者名主色小字＋兩行粗標＋說明。
- 多空對照：兩欄卡，標題旁小標籤（看多 rgba(255,93,93,.18) 底紅字、看空 rgba(47,212,138,.15) 底綠字）。
- 時間軸：3–4 欄 grid，`::before` 漸層橫線＋16px 圓點光圈。
- 區間圖（信件報告）：容器 `position:relative;height:46px`；一條 6px 漸層線；最低／最高價以 `left:6%`、`left:94%` 放標籤＋主色圓點；其他券商依 `6+88×(v−lo)/(hi−lo)%` 放白點。
- 數字橫條：`<em>` 寬度依比例（最大值＝100%）。

每頁骨架：`<link rel="stylesheet" href="base.css">` → 來源 `:root` → 兩個 `.glow` → `.wrap`（header、標題區、各 `.sec` 區塊、footer）。

## 4. 輸出與檢查

`render.py`（Chrome 在 /usr/bin/google-chrome；先 `fc-list :lang=zh` 確認 Noto CJK TC）：

```python
import sys,asyncio,os
from playwright.async_api import async_playwright
async def main(names):
    d=os.path.abspath(os.path.dirname(__file__))
    async with async_playwright() as p:
        b=await p.chromium.launch(executable_path="/usr/bin/google-chrome")
        pg=await b.new_page(viewport={"width":1080,"height":1920},device_scale_factor=3)
        for n in names:
            await pg.goto(f"file://{d}/{n}.html"); await pg.wait_for_timeout(400)
            print(n,"content height",await pg.evaluate("document.querySelector('.wrap').scrollHeight"))
            await pg.screenshot(path=f"{d}/{n}.png")
        await b.close()
asyncio.run(main(sys.argv[1:]))
```

流程：
1. 寫 pp.html、ss.html、vocus.html、mail.html，執行 `python3 render.py pp ss vocus mail`。
2. content height > 1920 → 溢出：縮句、刪次要條目。
3. Read 看每張：溢出、孤字換行、底部留白 >150px（從筆記補細節或加元件）、篇數／頻道數是否與實際收錄一致。
4. 輸出：3x 截圖存 `…_3240.png`；PIL `Image.open(...).convert('RGB').resize((1080,1920), Image.LANCZOS)` 存 `…_1080.png`。檔名主體 `YYYY-MM-DD_PressPlay`、`_Substack`、`_方格子`、`_信件報告`。
5. SendUserFile（display: render）一次送出四張 3240 版（使用者要在對話裡直接看到圖）；1080 版留著備用。

## 5. 回覆

短表格列四張的篇數、主軸與區塊；說明日期判斷（哪些前一天／隔天的文章沒放或以延伸標示）、信件報告附件頁讀了哪些部分；註明數字取自 Notion 筆記、未另行上網查證；提醒 LINE 傳送請選原始畫質。

## 6. 歸檔到 Notion「每日圖卡」

資料庫 `collection://26c6b4e4-c29a-4e27-b7a2-6557e285a6db`（在「每日報告」頁下）。每張圖卡新增一列：
- 標題：`YYYY-MM-DD｜來源｜大標`；日期＝目標日；套件＝訂閱四套件；來源＝該卡來源；主軸＝大標加副標一句；發文文案＝3–5 行可直接貼社群的文字（結尾「非投資建議」）；發布狀態＝待發。
- 圖卡欄：能上傳檔案就附 3240 版 PNG；工作環境連不到 Notion 上傳端點時，其餘欄位照寫、圖卡欄留空，並在回覆說明「圖檔未上傳，需手動拖入」。不要為了上傳反覆重試。
- 套件或來源的選項不存在時，先用 update-data-source 新增選項再寫入。
- 同一天同一來源已有列時更新該列，不要重複新增。

## 7. 排程（無人值守）執行

由排程啟動、使用者不在時：不要發問。某來源當天沒有資料就略過那一張，其餘照做；需要判斷的地方採最合理做法，並在回覆第一行寫明用了哪一天的資料、略過了什麼。Notion 查詢額度用完（usage_limit_reached）時不要重試同一查詢，改用 view 模式或 notion-search，並在回覆提一次。
