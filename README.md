# 每日圖卡產線

這個儲存庫只做一件事：讓 Claude 的雲端排程任務有地方讀取圖卡技能。

- `.claude/skills/` 底下是四個技能：`daily-three-pack`（社群三套件）、`daily-subscription-four-pack`（訂閱四套件）、`daily-av-two-pack`（影音雙套件）、`single-feature-card`（單篇精選卡）。
- 排程任務執行時會複製這個儲存庫，從 Notion「每日報告」讀資料、做成 9:16 圖卡，再歸檔到 Notion「每日圖卡」。
- 圖卡本身不會存進這裡；執行產生的檔案都是暫時的。

要調整版型或規則，改對應的 `SKILL.md` 即可，下一次執行就會生效。
