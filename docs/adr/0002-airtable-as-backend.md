# 改用 Airtable 取代 Google Sheets 作為資料庫後端

**狀態**：accepted

Google Sheets 沒有欄位型別與關聯驗證，且 Claude 只能讀不能寫。改用 Airtable，透過 MCP connector 讓 Claude 可直接讀寫。已建立 base「CCNA×Security+ 知識地圖」（appJYEybBNNACjevl），單一 Nodes 表，用 `Parent`／`Related` 兩個自我關聯的 Linked Record 欄位取代原本 Google Sheet 版本的純文字 `parent_id`／`related_ids`，換取關聯驗證與雙向可見性。

流程仍維持「單次任務式」（見另一則關於版本控制的決定）：手動請 Claude 讀取簡報並寫入 Airtable，尚未串接 Git 版本控制。

## Considered Options
- 繼續用 Google Sheets：放棄，因為缺乏欄位驗證且 Claude 無寫入權限
- Notion Database：放棄，其關聯欄位偏向頁面雙向連結，不如 Airtable 的 Linked Record 直覺
- 自架 Baserow/NocoDB：延後，對單人學習型專案的維運成本過高

## 附加決策：新增 `Flow Order` 欄位取代獨立的第三種 edge type
簡報中有多個「循序步驟型」圖表（NIST CSF 五大功能、IAAA 四步驟、三種控制功能類型時間軸），這類圖表不是單純的階層或跨領域關聯。沒有另外設計第三種 edge type，而是讓這些步驟節點共用同一個 `parent`（該圖表的總節點），並加上 `Flow Order` 數字欄位標示順序，之後由心智圖前端依此欄位畫出循序排列。
