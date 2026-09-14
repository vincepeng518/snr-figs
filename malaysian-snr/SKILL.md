# Skill: msnr-analyze (v4 XAU)

當使用者要求看盤、給圖、給水平、或輸入 /msnr 時執行。
默認黃金 XAUUSD。完整規則讀 `references/msnr-playbook.md`。

## 分析順序（不可跳）

### A. 日線 — 故事 + 軌道
1. 列出可見 Daily A / V / Gap（含 Unfresh）。每條標：價位、類型、origin/RBS/SBR、Fresh 或 Unfresh、現價上方/下方、wall / rail / road。
2. Daily Unfresh = **rail**，不是作廢。可以起偏見、可以繼續偏見。
3. Storyline：最近一次 Daily **body** 事件（收盤離開水平 = reject；實體穿越 = 翻轉）。不要求 reject 必須發生在 Fresh 上。
4. Daily bias：偏多 / 偏空 / 中性。代價 rail/wall 仍拔住價格就可以給方向；沒有 Fresh 目標時降低確信，不自動變 Neutral。
5. 目標：優先下一條 Daily Fresh wall。路上 Daily Unfresh 仍可能再 reject。
6. 失效：Daily 實體收盤打穿定故事的那條 rail/wall。

### B. H4 — M5 的父母
1. 列 H4 A/V/Gap。
2. 相對 Daily：順勢（continuation） / 路障回調（roadblock） / 衝突（conflict）。
3. 衝突 → 日內 = 觀望。M5 不得自創方向。
4. H4 Unfresh = road；只有貼在 Daily rail 上才可當進場理由。

### C. M5 — 黃金日內偏見（孩子）
1. 只標當前擴張/盤中最近 3 條。不存一週的 M5。
2. 只有 M5 **Fresh** 才能定 M5 偏見。M5 Unfresh 不進場、不投票。
3. 嵌套表（只能選一個日內結論）：

| Daily | H4 | M5 | 日內 | 允許 |
|---|---|---|---|---|
| 多 | 順 | 多 | 偏多 | 買 M5 Fresh V / Fresh RBS |
| 多 | 順 | 空 | 逢低 | 不空 M5；等 M5 Fresh V 踏入 H4/Daily |
| 多 | 路障 | 任意 | 逢低 | 只買 Daily/H4 軌道，M5 只計時 |
| 多/空 | 衝突 | 任意 | 觀望 | 無 |
| 空 | 順 | 空 | 偏空 | 賣 M5 Fresh A / Fresh SBR |
| 空 | 順 | 多 | 逢高 | 不多 M5 |
| 空 | 路障 | 任意 | 逢高 | 只賣父層軌道 |
| 中性 | 任意 | 任意 | 觀望 | 只記地圖 |

4. M5 偏見死亡：M5 實體打穿、H4 失效、Daily 失效、或 60 分鐘內有 CPI/FOMC/NFP。

### D. 進場（條件式）
允許位置：Daily Fresh wall；Daily Unfresh rail + H4 收盤離開；H4 Fresh；H4 Unfresh 僅當貼 Daily rail；順嵌套方向的 M5 Fresh。
禁止：M5 Unfresh；沒 Daily 摺疊的 H4 Unfresh；逆嵌套方向。

格式：
- 若價格到達 [水平] 且 [H4 或 M5 收盤離開] → 考慮 [多/空]
- 無效：該 TF 實體收盤穿越 [價位]
- TP1 同 TF 下一條反向 Fresh wall；TP2 父層 rail/wall
- 同一條 Daily rail 同一根 Daily K 不重複進場

## 強制輸出

### 1. 結論
- 品種 / 現價 / session / 60m 內新聞
- 日線偏見 + 作用中 rail/wall + 目標
- H4：順勢 / 路障 / 衝突
- M5 偏見（或 none）
- 日內：偏多 / 逢低 / 偏空 / 逢高 / 觀望
- 確信：高 / 中 / 低
- 有效至

### 2. Daily 地圖
walls / rails / 失效

### 3. H4 地圖
與 Daily 關係 / walls / roads

### 4. M5 地圖
Fresh walls；Unfresh = 不進場

### 5. 計畫（觀望則省）
IF … THEN … SL / TP1 / TP2

### 6. 職責
結構閱讀，不是預測，也不是投資建議。
