# CCNA × Security+ 知識地圖

用 Cytoscape.js + Airtable 做的可互動知識地圖，目前資料涵蓋 CompTIA Security+ SY0-701 Lesson 1。

## 這個 repo 裡有什麼

- `versions/` — 每個版本的 Cytoscape.js HTML 檢視器/編輯器（直接用瀏覽器打開即可）
- `docs/spec.md` — 目前為止定案的完整規格書
- `docs/adr/` — 架構決策紀錄（Architecture Decision Records）

## 資料庫

資料存在 Airtable（base 名稱：CCNA×Security+ 知識地圖），包含：
- `Nodes` 表：節點內容、樣式（顏色/形狀/大小）、座標
- `Hierarchy (教材課綱)` 表：依教材課綱分類的階層關係（parent-child）
- `Edges` 表：線段樣式覆寫（只記錄跟預設樣式不同的線）
- `Nodes_backup_before_v0.1`：v0.1 定案前的 Nodes 表快照備份

## 版本紀錄

- **v0.1**（本次提交）：Lesson 1 完整節點資料、心智圖檢視/編輯器（含 WASD 平移、線段編輯、拖曳鎖定）、Nodes/Hierarchy/Edges 三表分離的資料結構定案。
