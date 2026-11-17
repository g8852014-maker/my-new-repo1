---
name: "daily-av-two-pack"
description: "每日影音雙套件：從 Notion 的 Podcast 筆記（六個節目資料庫）與 YouTube 頻道筆記，取指定日（預設前一天）各做一張 9:16 PNG 圖卡。使用者說「每日影音雙套件」「影音雙套件」時使用。"
---

# 每日影音雙套件

產出：兩張 9:16 直式資訊圖卡（Podcast、YouTube），繁體中文、台灣用語。版面 1080×1920、3x 截圖；交付 3240×5760 PNG（另留 1080×1920 版）。**預設淺底深字**；使用者說「深色版」才用深色色票。資訊密度不要因為主題或解析度而減少。

目標日：使用者指定就用指定日（「今天的」＝當天，但晚間節目的筆記常要到深夜或隔天才進 Notion，當天做時要在回覆說明還缺哪些）；未指定用台北時間「前一天」（建議排程在早上 07:00 做前一天，資料才完整）。在回覆註明。

## 1. 抓資料（Notion）

用 ToolSearch 載入 `mcp__Notion__notion-fetch`、`mcp__Notion__notion-query-data-sources`、`mcp__Notion__notion-search`。

### Podcast（總頁 `3e1c68eecc0f81178746e0bc8e1cdfff`）
六個節目資料庫，欄位相同：Name、TLDR、主題、date:日期:start（純日期，即台北日）、狀態、集數、集標題。

| 節目 | collection | 預設 view（SQL 額度用完時用） | 上架規律 |
|---|---|---|---|
| 股癌 | `collection://5cc754e3-b559-4921-9413-bf050bbb3f72` | 先 fetch 資料庫 `f9abd1b40ddf4b0485c46fe54168a559` 取 view | 週三、六 |
| M觀點 | `collection://9a449f10-e4dc-42a6-a5e3-588112c3e923` | 資料庫 `c8a172cf22694666b55ec7c5c7171d7e` | 週一、四 |
| 韭菜畢業班 | `collection://316a536a-a4f8-4613-9de2-9e15ccf9e9d0` | `https://www.notion.so/aae2151dbf0b48779653225d04b52e57?v=5e1bc19532b44379927df0954cbcfaa6` | 週日、三 |
| 兆華與股惑仔 | `collection://ca853e43-1390-4d05-b76f-312e95107ba8` | 資料庫 `3ead185499424d409d46953f8d5852e7` | 平日 |
| 財經M平方 | `collection://b3537d7b-f6b5-4eab-a288-e31fd84d5228` | `https://www.notion.so/fc6ec0be44674710b2240f07e2015f75?v=fbd6f3f217474eeea34b8473827afc7c` | 週日 |
| All-In | `collection://020fbedb-8285-406f-9db7-1dd384506989` | `https://www.notion.so/b6ed0133d4bf4cb38fdecca426430e79?v=6a8664abd8164fdfb4eae2c3dae54f3c` | 週末＋不定期專訪 |

查詢：每個資料庫各一個 SQL（`SELECT url,"Name","集數","date:日期:start" d,"狀態","TLDR" FROM "collection://…" ORDER BY d DESC LIMIT 3`），並行送出。
- Notion 免費方案的 SQL／rows 查詢有額度；若回傳 usage_limit_reached，不要重試同一查詢，改用 view 模式：`{"mode":"view","view_url":"<上表 view>","page_size":100}`（參數直接放在 data 內，不要再包一層 data）。表中只寫資料庫 ID 的，先 fetch 該資料庫取得 `view://` ID 再組 view_url。回覆時依工具錯誤訊息的指引提一次額度狀況。
- 只取 `狀態=已整理` 且日期＝目標日的單集；「流程驗證樣本／非新集」不收。
- 目標日的幾集 fetch 全文（讀「30 秒重點」「投資人重點」即可，不必讀逐字稿）。
- 目標日只有 1–2 集時，前 1–2 天的新集放在「延伸」區（只用 TLDR 寫一句話），區塊標題要寫明日期。

### YouTube 頻道筆記（頁面 `3f7c68eecc0f814da237de4ee72435de`）
- 是一個頁面、子頁最新在上，標題 `YYYY-MM-DD｜頻道｜標題`。fetch 後挑日期＝目標日的子頁全部 fetch。
- 子頁若太長被存成檔案，用 python 取 `<content>` 開頭約 9,000 字（「重點摘要」都在前段）；回覆說明只讀摘要。
- 注意筆記日期與 YouTube 上架日可能不同（看 meta「YouTube 上架日期」），不同時在圖上標「影片 M/D 上架」。

## 2. 編輯原則

- 主軸：綜合當天各集／各影片的共同結論，寫成兩行有判斷的大標（關鍵詞用 `<em>` 主色）＋一行副標。
- 每集／每支一張內卡：節目名當區塊標題，主持人或來賓用主色小標籤＋一句集標題；4 格 KPI（只用筆記裡的數字）＋2–4 條重點。
- 節目內容都是口述：主持人判斷要寫成他的看法；筆記標「推論／待查證／非承諾／疑似業配」的加 `<span class="tag">`；業配段落不放上圖。
- 可用的視覺元件（依內容挑）：
  - 三層分級框（已證實／據報導／沒有證據，或機率高／中／尾部風險）。
  - 事件時間軸（5 欄；未發生的點用虛線圓）。
  - 黃金切割／位階計：橫線上標前低、高點與關鍵點，位置依數值比例算；下一個目標點標在線下以免重疊。
  - 小長條圖（如護盤天數）：固定容器高度、每根用 px 高度。
  - 雙向橫條（近期跌幅 vs 今年漲幅）：左綠右紅，長條最長只占欄寬約 75% 以免蓋到數字；兩側比例尺不同時要註明。
  - 標的胶囊（如國安常買名單）。
  - 延伸區 2×2 小卡（節目名主色＋一句話）。
- 頁尾：左「來源：Notion ○○筆記（節目口述，未另查證）」，右「非投資建議」。

## 3. 設計系統

與「每日社群三套件」「每日訂閱四套件」同一套：深色底、Noto Serif CJK TC 900 標題、Noto Sans CJK TC 內文 500 字重、台股慣例紅漲綠跌（`--up:#ff5d5d;--dn:#2fd48a`）。防毛邊：內文全用 500 字重、用下方調亮過的灰、文字最小 17px、交付前自行 Lanczos 縮成 1080 寬。

深色色票（使用者指定「深色版」時）：
- Podcast：`--bg:#0b0f08;--card:#141b10;--line:#2a3622;--ink:#f5f9ef;--ink2:#e3eadb;--mute:#b4bfa9;--acc:#c4f04d`，badge「🎙」，光暈 #4d6a12 右上、#1e4a3a 左下
- YouTube：`--bg:#080d14;--card:#111926;--line:#22304a;--ink:#f2f6fb;--ink2:#e0e7f0;--mute:#adb7c6;--acc:#5cc8ff`，badge「▶」，光暈 #1a5a8a 右上、#3a2a6a 左下

淺色色票（預設）：
- Podcast：`--bg:#f6f9ef;--card:#ffffff;--line:#d9e3c6;--ink:#131a08;--ink2:#28301a;--mute:#57614a;--acc:#4c7a00`，光暈 #dcf0a8 右上、#c5ead9 左下
- YouTube：`--bg:#f4f7fb;--card:#ffffff;--line:#d3dce8;--ink:#0b1524;--ink2:#1f2b3d;--mute:#4f5b6d;--acc:#0a6fc2`，光暈 #bfe0fb 右上、#ddd3f5 左下

**淺色主題的追加樣式**（接在 base.css 後或各頁 `<style>`）：

```css
body{--up:#d23131;--dn:#0f8a55}
.badge{color:#fff}
.card,.kpi{box-shadow:0 2px 10px rgba(20,30,60,.06)}
.tag{background:var(--bg)}
```
光暈改用下列淺色、`.glow{opacity:.55}`。深色主題沿用原本 `--up:#ff5d5d;--dn:#2fd48a`。
淺色主題另加 `.ep b{color:#fff}`；卡片內的 KPI 用 `background:var(--bg)`、不加陰影。

**交付前多檢查三件事**：①圖表標籤彼此或與圓點、長條重疊（放大局部看，重疊就併成一行或移位）；②KPI 標題或數值換行後只剩一個字（縮短標題或調小字級）；③來源寫在事件之前的內容要標出來（如「盤前」「報告發出時未反映當日收盤」），免得被讀成對事件的回應。

共用 `base.css`（寫入工作目錄 `cards/`）：

```css
*{margin:0;padding:0;box-sizing:border-box}
html,body{width:1080px;height:1920px;overflow:hidden}
body{font-family:"Noto Sans CJK TC",sans-serif;background:var(--bg);color:var(--ink);position:relative;text-rendering:geometricPrecision}
.wrap{position:absolute;inset:0;padding:56px 60px 44px;display:flex;flex-direction:column;gap:15px}
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
.card{background:var(--card);border:1px solid var(--line);border-radius:22px;padding:16px 22px;position:relative}
.card p{font-weight:500}
.hl{color:var(--acc);font-weight:700}
.up{color:var(--up);font-weight:700}.dn{color:var(--dn);font-weight:700}
.tag{display:inline-block;font-size:19px;padding:2px 10px;border-radius:8px;border:1px solid var(--line);color:var(--mute);margin-left:8px;vertical-align:3px;font-weight:500}
footer{margin-top:auto;display:flex;justify-content:space-between;font-size:19px;color:var(--mute);border-top:1px solid var(--line);padding-top:14px;font-weight:500}
.grid{display:grid;gap:14px}
.kpi{background:var(--card);border:1px solid var(--line);border-radius:18px;padding:10px 12px}
.kpi .k{font-size:18px;color:var(--mute);font-weight:500;line-height:1.35}
.kpi .v{font-size:26px;font-weight:900;line-height:1.2;color:var(--acc)}
.k4{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:10px}
.ep{display:flex;align-items:center;gap:12px;margin-bottom:10px}
.ep b{font-size:19px;color:var(--bg);background:var(--acc);border-radius:8px;padding:2px 10px}
.ep span{color:var(--ink);font-weight:700;font-size:24px}
ul.dot{list-style:none;display:flex;flex-direction:column;gap:8px}
ul.dot li{font-size:21.5px;line-height:1.5;color:var(--ink2);padding-left:24px;position:relative;font-weight:500}
ul.dot li::before{content:"";position:absolute;left:2px;top:14px;width:9px;height:9px;border-radius:2px;background:var(--acc);transform:rotate(45deg)}
ul.dot li b{color:var(--ink)}
```

每頁骨架：`<link rel="stylesheet" href="base.css">` → 來源 `:root` → 兩個 `.glow` → `.wrap`（header、標題區、各 `.sec`＋`.card`、footer）。

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
1. 寫 pod.html、yt.html，執行 `python3 render.py pod yt`。
2. content height > 1920 → 溢出：縮句、刪次要條目。
3. Read 看每張：溢出、孤字換行、底部留白 >150px（從筆記補內容或加元件）、圖表標籤是否和長條或圓點重疊（重疊就放大局部檢查後修）。
4. 輸出：3x 截圖存 `…_3240.png`；PIL `Image.open(...).convert('RGB').resize((1080,1920), Image.LANCZOS)` 存 `…_1080.png`。檔名主體 `YYYY-MM-DD_Podcast`、`YYYY-MM-DD_YouTube`。
5. SendUserFile（display: render）一次送出兩張 3240 版（使用者要在對話裡直接看到圖）；1080 版留著備用。

## 5. 回覆

短表格列兩張的集數／影片數、主軸與區塊；說明哪些是前幾天的延伸、哪支影片上架日不同；註明內容為節目口述、未另行查證；若遇到 Notion 查詢額度限制，依錯誤訊息指引提一次；提醒 LINE 傳送請選原始畫質。

## 6. 歸檔到 Notion「每日圖卡」

資料庫 `collection://26c6b4e4-c29a-4e27-b7a2-6557e285a6db`（在「每日報告」頁下）。每張圖卡新增一列：
- 標題：`YYYY-MM-DD｜來源｜大標`；日期＝目標日；套件＝影音雙套件；來源＝該卡來源；主軸＝大標加副標一句；發文文案＝3–5 行可直接貼社群的文字（結尾「非投資建議」）；發布狀態＝待發。
- 圖卡欄：能上傳檔案就附 3240 版 PNG；工作環境連不到 Notion 上傳端點時，其餘欄位照寫、圖卡欄留空，並在回覆說明「圖檔未上傳，需手動拖入」。不要為了上傳反覆重試。
- 套件或來源的選項不存在時，先用 update-data-source 新增選項再寫入。
- 同一天同一來源已有列時更新該列，不要重複新增。

## 7. 排程（無人值守）執行

由排程啟動、使用者不在時：不要發問。某來源當天沒有資料就略過那一張，其餘照做；需要判斷的地方採最合理做法，並在回覆第一行寫明用了哪一天的資料、略過了什麼。Notion 查詢額度用完（usage_limit_reached）時不要重試同一查詢，改用 view 模式或 notion-search，並在回覆提一次。
