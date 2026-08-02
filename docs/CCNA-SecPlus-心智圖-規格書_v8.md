# CCNA × Security+ 知識地圖 — 規格書

版本：對應心智圖輸出 `lesson1-v8`　｜　最後更新：本次對話彙整

## 1. 專案目的

一份可互動導覽的知識地圖，用 Cytoscape.js 整合 CCNA（網路工程）與 Security+（資訊安全）兩張證照的知識體系，資料庫用 Airtable，目的是搭配教材（目前為 CompTIA Security+ SY0-701 簡報）邊複習邊檢視知識結構。後續會再納入 Coursera Google Cybersecurity 課程內容與 CCNA 網路拓樸圖，但目前優先完成 Security+ 部分。

## 2. 資料庫（Airtable，base：CCNA×Security+ 知識地圖）

目前共五張表，各司其職：

| 表名 | 用途 |
|---|---|
| `Nodes` | 節點的內容本體（標題、內文、比較表格、視覺樣式、座標） |
| `Hierarchy (教材課綱)` | 依教材課綱分類的階層關係（parent-child），與 Nodes 分離 |
| `Edges` | 線段樣式覆寫，只有手動調整過的線才有列，其餘沿用預設樣式 |
| `Dictionary` | Security+ 官方詞彙表（606 筆），供心智圖內文的詞彙螢光標記／點擊查詢用 |
| `Nodes_backup_before_v0.1` | v0.1 定案前 Nodes 表的純文字快照備份 |

### 2.1 Nodes 表欄位總覽

| 欄位 | 型別 | 用途 |
|---|---|---|
| Label | 單行文字 | 節點顯示名稱，格式：英文 → 中文 →（括號補充），換行分隔 |
| Node Key | 單行文字 | 跨表穩定識別碼（如 `l1-confidentiality`） |
| Content | 多行文字 | 節點內文說明；會被詞彙螢光標記掃描 |
| Table Diagram Data | 多行文字 | 比較表格（markdown）或流程圖描述文字 |
| Flow Order | 數字 | 流程圖節點的步驟順序 |
| Hyperlink | URL | 外部參考連結 |
| Source Slide | 單行文字 | 來源簡報與頁碼 |
| Category | 單選 | 領域分類，尚未填入 |
| Color Override | 單行文字 | 節點顏色，格式 `顏色名稱 (色碼)` |
| 形狀 Shape | 單選（25 選項，中文） | 節點形狀，對應 Cytoscape.js 內建形狀 |
| Size | 數字 | 節點大小 |
| Status | 單選 | 個人讀書進度，尚未使用 |
| Related | Linked Record（自我關聯） | 跨段落／跨主題關聯 |
| Pos X / Pos Y | 數字 | 心智圖畫布座標 |
| Hierarchy (Parents) | Linked Record（自動反向欄位） | 連到本節點在 Hierarchy 表裡對應的那一列 |
| Hierarchy (Children) | Linked Record（自動反向欄位） | 連到以本節點為 Parent 的所有 Hierarchy 列，即子節點 |

> Nodes 表本身**不含**階層歸屬欄位；舊的 `Parent`／`Children` 欄位已由使用者刪除，階層改存在 `Hierarchy (教材課綱)` 表。

### 2.2 Hierarchy (教材課綱) 表欄位

| 欄位 | 型別 | 用途 |
|---|---|---|
| Node Key | 單行文字（主鍵） | 與 Nodes 表的 Node Key 一致 |
| Node | Linked Record → Nodes | 這一列代表哪個節點 |
| Parent Node | Linked Record → Nodes | 在此分類方式下的上層節點，根節點留空 |

未來若要新增分類方式（例如「攻與防」），做法是**再開一張同樣格式的新表**，Nodes 表完全不用修改。

### 2.3 Edges 表欄位

| 欄位 | 型別 | 用途 |
|---|---|---|
| Edge Key | 單行文字 | 格式 `{parent_key}__{child_key}`，對應心智圖 HTML 內的 edge id |
| Curve Style / Line Style / Width / Line Color / Label | — | 線段樣式覆寫，只有手動調整過的線才有列 |

### 2.4 Dictionary 表欄位

| 欄位 | 型別 | 用途 |
|---|---|---|
| English Term | 單行文字（主鍵） | 縮寫或完整英文詞彙 |
| Chinese Term | 單行文字 | 原文括號內的中文翻譯 |
| Description | 多行文字 | 詞彙說明 |

606 筆，來源為使用者提供的 Security+ SY0-701 官方詞彙表 PDF（OCR 掃描件）。**編輯策略**：這張表變動頻率低，使用者需要時會直接在 Airtable 手動修改；Claude 不會每次出新版都主動檢查這張表，只有使用者明確要求時才核對。

### 2.5 已知工具限制

Claude 目前使用的 Airtable 工具沒有辦法：
- 直接編輯既有 Single Select 欄位的選項名稱
- 刪除單一欄位（只能刪除整張表；欄位刪除需使用者手動操作）

## 3. 視覺呈現規則

### 3.1 節點名稱（Label）格式

統一順序：**英文 → 中文 →（括號補充）**，每段用換行分隔。

### 3.2 節點大小

依階層深度分五級：root 最大（100），每往下一層遞減（75/58/44/34），使用者可個別覆蓋。

### 3.3 顏色與形狀

由使用者手動指定，欄位格式：
- 顏色：`顏色名稱 (色碼)`，例如 `blue (#2196F3)`；若來自調色盤手動選色則寫 `custom (色碼)`
- 形狀：中文選單，對應 Cytoscape.js 內建形狀（詳見附錄 A）

### 3.4 兩種邊（Edge）

| 類型 | 樣式 | 意義 |
|---|---|---|
| Hierarchy（階層邊） | 實線、預設跟隨 parent 節點顏色、較粗，帶箭頭 | 對應 Hierarchy (教材課綱) 表的 Parent Node |
| Related（跨段落關聯） | 不在畫布上畫出，只在側邊欄以按鈕呈現 | 對應 Nodes 表的 Related 欄位 |

> 決策紀錄：曾考慮用 Cytoscape compound node 做分組框，因 `Overlap` 節點需跨兩領域而放棄（ADR-0001）。

## 4. 心智圖檢視器（HTML/Cytoscape.js）功能

### 4.1 基本瀏覽
- 節點依 Airtable 資料渲染顏色、形狀、大小、位置（preset layout）
- 點節點開啟右側側邊欄：來源頁碼、內文（含詞彙螢光標記）、比較表格
- 側邊欄依序顯示：上層節點 → 下層節點 → 跨段落關聯，皆為可點擊跳轉按鈕
- 左上角搜尋框：輸入關鍵字，不符合的節點會淡化
- **WASD 平滑連續平移**（按住持續移動，非逐格跳動）；滑鼠滾輪縮放

### 4.2 詞彙螢光標記與查詢（v8 新增）
- 節點內文（Content）中，只要出現 Dictionary 表 English Term 欄位裡的詞彙，會自動套上淡紫色螢光底色（`rgba(196,160,255,0.28)`）
- 滑鼠移到螢光詞彙上，底色變亮紫（`rgba(196,160,255,0.55)`）
- 點擊螢光詞彙，在滑鼠位置附近彈出小懸浮框，顯示該詞的 Description 全文；點擊懸浮框以外的地方即關閉
- 比對規則：不分大小寫、以完整單字邊界比對（例如 `MAC` 不會誤配到 `MAC filtering` 這種更長詞彙的一部分，因為比對時較長詞彙優先）
- 資料只涵蓋 Dictionary 表的 English Term 欄位，Chinese Term 目前不參與比對

### 4.3 編輯模式
| 操作 | 說明 |
|---|---|
| 🔒／🔓 鎖頭圖示 | 切換編輯模式，開啟時畫布外框亮起提示 |
| 點節點（編輯模式下） | 側邊欄切換成編輯表單：名稱、內文、形狀、顏色、大小 |
| 點線段（編輯模式下） | 側邊欄切換成線段編輯表單：curve-style、line-style、寬度、顏色、線上文字、「與 parent node 同步顏色」勾選框；**閱覽模式下點線段完全沒有反應** |
| 拖曳節點 | **只有編輯模式下**可拖動節點；閱覽模式下節點鎖定，無法誤觸移動 |
| Ctrl+Z / Cmd+Z | 復原上一步編輯（節點資料、節點座標、線段樣式皆可復原） |
| ⬇ 匯出按鈕 | 產生本次所有異動的 JSON 並下載，含節點異動（`changes`，可能包含 `x`/`y` 座標）與線段異動（`edge_changes`） |

**線段顏色同步規則**：線段預設「與 parent node 同步」勾選，顏色自動跟隨來源節點；使用者手動改節點顏色時，仍勾選同步的子線段會一起變色。只要使用者透過調色盤手動指定某條線的顏色，該線的同步會自動取消勾選，其他線不受影響。線上文字的字體顏色固定，與線段本身顏色無關。

**匯出 JSON 格式：**
```json
{
  "version": "lesson1-v8",
  "exported_at": "2026-08-02T10:24:14.786Z",
  "changes": {
    "l1-confidentiality": { "color": "#1E88E5", "x": 631.6, "y": 772 }
  },
  "edge_changes": {
    "l1-root__l1-topic1a": { "lineColor": "#ffffff", "syncColor": false }
  }
}
```

### 4.4 目前的限制
- 編輯模式無法調整 Parent／Related 關聯（需要對照 Airtable Record ID，瀏覽器端無法安全處理；階層關係目前只能在 Airtable 的 `Hierarchy (教材課綱)` 表格檢視裡直接編輯）
- 線段樣式的異動目前只存在匯出的 JSON 裡；Airtable 的 `Edges` 表需要 Claude 讀取 JSON 後手動寫入，不是即時同步
- 匯出的 JSON 需要使用者上傳給 Claude 才會寫回 Airtable

## 5. 工作流程（目前階段）

1. 使用者上傳教材簡報（PDF）或詞彙表
2. Claude 抽取重點成節點／詞彙，寫入對應的 Airtable 表
3. Claude 產出 Cytoscape.js HTML 供使用者複習與檢視
4. 使用者一邊複習一邊在 Airtable 或心智圖編輯模式調整樣式／內容，用「⬇ 匯出」把異動交給 Claude 寫回 Airtable
5. Dictionary 表的維護獨立於上述流程：使用者需要時直接在 Airtable 手動改，不併入每次出新版的例行檢查

## 6. 版本控制（Git）

已於 v0.1 導入。GitHub repo：`p90p90p90/Mind-Maps`，`versions/` 資料夾存放每一版 HTML，`docs/` 存放規格書與 ADR。版號規則：每次有實質內容變動 +0.1（v0.1 → v0.2 → …），重大資料結構變動時可跳號（例如未來新增攻防分類視圖）。

## 7. 暫不實作／待討論事項

- 編輯模式中支援調整 Parent／Related 關聯
- CCNA 內容與 Google Cybersecurity 課程內容的節點（尚未開始）
- Status（讀書進度）欄位的視覺呈現方式
- **是否要把 Color Override／形狀 Shape／Size／Pos X／Pos Y 從 Nodes 表移到 Hierarchy (教材課綱) 表**，讓 Nodes 表只保留跟呈現方式無關的純內容（討論中，見對話紀錄）
- Dictionary 螢光標記未來是否要擴及 Table Diagram Data 或側邊欄以外的區域

## 附錄 A：Cytoscape.js 節點形狀完整列表

橢圓形｜三角形｜圓角三角形｜矩形｜圓角矩形｜下圓角矩形｜切角矩形｜桶形｜平行四邊形｜右斜平行四邊形｜菱形｜圓角菱形｜五邊形｜圓角五邊形｜六邊形｜圓角六邊形｜凹六邊形｜七邊形｜圓角七邊形｜八邊形｜圓角八邊形｜星形｜標籤形｜圓角標籤形｜V字形

## 附錄 B：ADR 索引

- ADR-0001：用扁平顏色分類取代 Compound Node 分組框
- ADR-0002：改用 Airtable 取代 Google Sheets 作為資料庫後端
- ADR-0003：把階層關係從 Nodes 表拆到獨立的 Hierarchy 表（依分類方式各開一張）
