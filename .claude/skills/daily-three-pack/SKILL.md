---
name: "daily-three-pack"
description: "每日社群三套件：從 Notion 每日社群消息抓今天的 grokbot-X、Threads、臉書三份匯整，各做一張詳細、有設計感的 9:16 PNG 圖卡。使用者說「每日社群三套件」「社群三套件」「每日三套件」時使用。"
---

# 每日社群三套件

產出：三張 9:16 直式資訊圖卡，分別對應今天（台北時間）的 X（grokbot）、Threads、臉書消息匯整。繁體中文、台灣用語。版面 1080×1920、3x 截圖；交付 3240×5760 PNG（另留 1080×1920 版備用）。**預設深色底**（使用者 2026-11-16 指定社群三套件用深色）；使用者說「淺色版」才用淺色色票。使用者若指定日期就用該日。

## 1. 抓資料（Notion）

先用 ToolSearch 載入 `mcp__Notion__notion-fetch`、`mcp__Notion__notion-search`。以台北時間取得今天日期（YYYY-MM-DD）。

三個總頁（子頁較新在上）：
- grokbot-X：`3d4c68eecc0f81dda483cba498cf68d0`（子頁名 `YYYY-MM-DD｜每日grokbot-X消息匯整`，週一為「（週一精簡版）」、週末為「（週末版）」）
- Threads：`3edc68eecc0f8100bffcc2817f9c6b88`（`YYYY-MM-DD｜每日Threads消息匯整`）
- 臉書：`406c68eecc0f8129a38ae565552ecc7c`（`YYYY-MM-DD｜每日臉書消息匯整`）

fetch 三個總頁，取標題日期＝目標日的子頁並 fetch 全文。若 ID 失效，用 notion-search 搜「每日社群消息」重新找入口。若某一份今天還沒出，告訴使用者哪一份缺，並詢問要用最新一期還是先做其他兩張；使用者不在時用最新一期並在回覆註明日期。

## 2. 編輯原則（每張卡）

- 主軸：取報告的「今日最重要／核心焦點／今日重點」，寫成兩行有判斷的大標（第二行關鍵詞用主色 `<em>`），加一行副標說明為什麼。
- 數字：優先放收盤、漲跌幅、財報、籌碼等硬數字做 KPI 格；只用報告裡有的數字，不自行補、不換算出報告沒有的數。
- 保留可信度標記：原文標「未經查證／單一媒體／貼文所述／待證／推測」的，圖上加 `<span class="tag">…</span>`；作者推估要標明是推估。
- 不放：梗文、政治澄清、純圖片／直播無文字、生活文。
- 原文沒給的資訊（如年份）不要自己補標籤，改用 ①②③ 等中性標示。
- 每張卡建議區塊（依當日內容取捨）：
  - X：收盤 6 格 KPI（指數、輝達等焦點股、原油、VIX）→ 今日最重要 3 件事（編號＋數據）→ 其他重點條列 → 行事曆（台北時間）→ 下一個關鍵數據的雙情境判讀框（綠框利多／紅框利空）。
  - Threads：台股大字 KPI＋籌碼三列（外資／融資／政府資金）→ 名家觀點卡 → 主事件時間軸（4 點）→ 美股／總經／AI 四格卡 → 台股族群亮點條列。
  - 臉書：每位重點粉專一個「焦點」區（觀點拆成編號 1-2-3；財報做 KPI 格＋比例長條；趨勢數字做小長條圖）→ 其他粉專短評條列。
- 頁尾：左「來源：Notion ○○匯整（補充說明）」，右「非投資建議」。

## 3. 設計系統

深色底、平台各一主色；標題 Noto Serif CJK TC 900，內文 Noto Sans CJK TC；台股慣例紅漲綠跌（`.up` 紅、`.dn` 綠）。

**防毛邊規則**（圖經 App、LINE、IG 縮圖或 JPEG 壓縮後，細字與暗灰字會糊）：內文、註記、表格一律 500 字重；`--ink2`、`--mute` 使用下方調亮過的色票；文字最小 19px；交付前自行用 Lanczos 縮成 1080 寬。

平台色票（寫在各 HTML 的 `:root`）：
- X：`--bg:#07090d;--card:#10151d;--line:#232c3a;--ink:#f3f6fa;--ink2:#e0e5ec;--mute:#aeb6c2;--acc:#3ee0c8`，badge「X」，光暈 #0f6f66 右上、#1b2f6b 左下
- Threads：`--bg:#120c14;--card:#1c1420;--line:#33263a;--ink:#fbf4f6;--ink2:#ebe2e7;--mute:#bfb2bb;--acc:#ff7a59`，badge「@」，光暈 #7a2c3a 左上、#4a2366 右下
- 臉書：`--bg:#0a0f1e;--card:#121a2e;--line:#24304d;--ink:#f4f6fb;--ink2:#e1e5ee;--mute:#b2b8c8;--acc:#f2c14e`，badge「f」，光暈 #1d3a8a 右上、#6b4a10 左下
- 三張共用：`--up:#ff5d5d;--dn:#2fd48a`

淺色色票（使用者指定「淺色版」時）：
- X：`--bg:#f2f8f7;--card:#ffffff;--line:#cfe2df;--ink:#08181a;--ink2:#1c3033;--mute:#4c6163;--acc:#0a8576`，光暈 #bdebe4 右上、#cfd9f7 左下
- Threads：`--bg:#fcf5f3;--card:#ffffff;--line:#ecd9d3;--ink:#20110d;--ink2:#36241f;--mute:#6b5650;--acc:#c8401f`，光暈 #ffd3c7 左上、#e6d5f5 右下
- 臉書：`--bg:#f5f7fc;--card:#ffffff;--line:#d6dcec;--ink:#0c1226;--ink2:#212840;--mute:#525a72;--acc:#9a6b00`，光暈 #cbd8fb 右上、#f5e3b5 左下

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
.wrap{position:absolute;inset:0;padding:56px 60px 44px;display:flex;flex-direction:column;gap:18px}
.glow{position:absolute;border-radius:50%;filter:blur(120px);opacity:.55;pointer-events:none}
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
.card p,td,.cal .x{font-weight:500}
.item{display:grid;grid-template-columns:62px 1fr;gap:6px 18px;align-items:start}
.num{font-family:"Noto Serif CJK TC";font-size:52px;font-weight:900;color:var(--acc);line-height:1}
.item h3{font-size:31px;font-weight:900;line-height:1.3}
.item p{grid-column:2;font-size:23px;line-height:1.55;color:var(--ink2);font-weight:500}
.hl{color:var(--acc);font-weight:700}
.up{color:var(--up);font-weight:700}.dn{color:var(--dn);font-weight:700}
.tag{display:inline-block;font-size:19px;padding:2px 10px;border-radius:8px;border:1px solid var(--line);color:var(--mute);margin-left:8px;vertical-align:3px;font-weight:500}
footer{margin-top:auto;display:flex;justify-content:space-between;font-size:19px;color:var(--mute);border-top:1px solid var(--line);padding-top:14px;font-weight:500}
.grid{display:grid;gap:14px}
.kpi{background:var(--card);border:1px solid var(--line);border-radius:18px;padding:10px 18px}
.kpi .k{font-size:20px;color:var(--mute);font-weight:500}
.kpi .v{font-size:34px;font-weight:900;line-height:1.2}
.kpi .d{font-size:22px;font-weight:700}
ul.dot{list-style:none;display:flex;flex-direction:column;gap:9px}
ul.dot li{font-size:24px;line-height:1.5;color:var(--ink2);padding-left:24px;position:relative;font-weight:500}
ul.dot li::before{content:"";position:absolute;left:2px;top:15px;width:9px;height:9px;border-radius:2px;background:var(--acc);transform:rotate(45deg)}
ul.dot li b{color:var(--ink)}
```

可加的元件（寫在各頁 `<style>`）：
- 時間軸：3–4 欄 grid，`::before` 橫線漸層到主色，每點 16px 圓點＋光圈。
- 比例長條：`grid-template-columns:62px 1fr 150px`，`<em>` 寬度＝占比%。
- 小長條圖：容器固定高度（如 190px），每根用 px 高度（不要用 % 高度，flex 欄內會失效），目標值用虛線斜紋。
- 雙情境框：左綠（rgba(47,212,138,.1) 底＋.35 邊）、右紅（rgba(255,93,93,.1)）。
- 注意 `.flow b{color}` 這類規則會蓋掉 `.up/.dn`，需加更具體選擇器。

每頁骨架：`<link rel="stylesheet" href="base.css">` → 平台 `:root` → 兩個 `.glow` → `.wrap`（header、標題區、各 `.sec` 區塊、footer）。

## 4. 輸出與檢查

`render.py`（Chrome 在 /usr/bin/google-chrome；先 `fc-list :lang=zh` 確認有 Noto CJK TC）：

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
1. 寫 x.html、threads.html、fb.html，執行 `python3 render.py x threads fb`。
2. content height > 1920 → 內容溢出：縮短句子、刪次要條目、減少副標行數，重算。
3. 用 Read 看每張 PNG：檢查溢出、孤字換行（如單獨一個「高。」）、底部大片留白（>150px 就從原報告補一則細節或加一個元件）、顏色是否套對、圖表比例是否正確。
4. 輸出：3x 截圖存 `YYYY-MM-DD_X消息_3240.png` 等；再用 PIL `Image.open(...).convert('RGB').resize((1080,1920), Image.LANCZOS)` 存 `…_1080.png`。檔名主體 `YYYY-MM-DD_X消息`、`YYYY-MM-DD_Threads消息`、`YYYY-MM-DD_臉書消息`。
5. 用 SendUserFile（display: render）一次送出三張 3240 版（使用者要在對話裡直接看到圖）；1080 版留著備用。

## 5. 回覆

短表格列三張的主軸與區塊；註明數字取自 Notion 報告、未另行上網查證（若有查證則註明來源與日期）；列出任何自行處理的標示（如年份未知改用①②③）；提醒 LINE 傳送請選原始畫質；最後一行提供可調整項（淺色版、品牌浮水印、拆輪播）。

## 6. 歸檔到 Notion「每日圖卡」

資料庫 `collection://26c6b4e4-c29a-4e27-b7a2-6557e285a6db`（在「每日報告」頁下）。每張圖卡新增一列：
- 標題：`YYYY-MM-DD｜來源｜大標`；日期＝目標日；套件＝社群三套件；來源＝該卡來源；主軸＝大標加副標一句；發文文案＝3–5 行可直接貼社群的文字（結尾「非投資建議」）；發布狀態＝待發。
- 圖卡欄：能上傳檔案就附 3240 版 PNG；工作環境連不到 Notion 上傳端點時，其餘欄位照寫、圖卡欄留空，並在回覆說明「圖檔未上傳，需手動拖入」。不要為了上傳反覆重試。
- 套件或來源的選項不存在時，先用 update-data-source 新增選項再寫入。
- 同一天同一來源已有列時更新該列，不要重複新增。

## 7. 排程（無人值守）執行

由排程啟動、使用者不在時：不要發問。某來源當天沒有資料就略過那一張，其餘照做；需要判斷的地方採最合理做法，並在回覆第一行寫明用了哪一天的資料、略過了什麼。Notion 查詢額度用完（usage_limit_reached）時不要重試同一查詢，改用 view 模式或 notion-search，並在回覆提一次。
