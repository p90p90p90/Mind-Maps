# 把階層關係從 Nodes 表拆到獨立的 Hierarchy 表（依分類方式各開一張）

**狀態**：accepted

隨著未來要支援多種分類視圖（例如目前的「教材課綱」、之後的「攻與防」），Nodes 表原本內建的 `Parent` 欄位無法同時代表多種階層——一個節點在不同分類方式下會有不同的上層節點。

決定把階層關係抽出成獨立的 `Hierarchy (教材課綱)` 表，欄位為 `Node Key`（文字）、`Node`（Linked Record → Nodes）、`Parent Node`（Linked Record → Nodes，根節點留空）。Nodes 表本身完全不含階層資訊，未來新增分類方式時，只需要再開一張同樣格式的表（例如 `Hierarchy (攻與防)`），不需要修改 Nodes 表結構。

執行前先把 Nodes 表完整快照備份到 `Nodes_backup_before_v0.1`（純文字攤平版，不含關聯欄位型別），以防拆分過程中出錯可以回頭核對。

## Considered Options
- 在 Nodes 表開多個 Parent 欄位（Parent（依課程）、Parent（依攻防）…）：放棄，因為每加一種分類方式就要幫 Nodes 表加欄，長期會讓 Nodes 表過度肥大，且不容易一眼看出目前使用中的是哪一欄
- 附帶效益：把階層變成「表格列」之後，使用者可以直接在 Airtable 的表格檢視裡新增/修改/篩選階層關係，不需要額外做拖曳式的階層編輯 UI
