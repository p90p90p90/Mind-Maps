# 用扁平顏色分類取代 Compound Node 分組框

**狀態**：accepted

心智圖需要呈現「這個節點屬於哪個領域（CCNA / Security+ / 跨領域）」。考慮過 Cytoscape.js 原生的 compound node（畫出紅/藍底色大框圈住節點），但 compound node 是嚴格樹狀結構，一個節點只能有一個 parent，無法表達 `overlap`（跨領域，如 VPN、Zero Trust）這種同時屬於兩邊的節點。

決定維持現有 demo 的做法：用節點 `category` 決定的扁平顏色分類（root/ccna/secplus/overlap）搭配 `related` 虛線邊來表達跨領域關聯，不使用 compound node 畫外框。

## Considered Options
- Compound node 分組框：放棄，因為與 `overlap` 分類的資料語意衝突
- 手動疊加半透明色塊（非 Cytoscape 原生）：延後，等有明確的「強調某個子集合」需求（例如複習範圍標記）再評估
