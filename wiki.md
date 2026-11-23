# UPS 供電接線圖入門 Wiki

主題：以實驗室 30 kVA UPS 供電系統為例，說明電從哪裡來、怎麼讀圖、怎麼驗算。
簡報：https://c24115015.github.io/ups-wiring-tutorial/

## 來源頁

| ID | 來源 | 類型 | 可信度 |
|---|---|---|---|
| S-01 | HPH-30KVA 供電示意圖（QElectroTech 專案檔，自製） | 第一手 | 高 |
| S-02 | Vertiv：What are the different types of UPS systems<br>https://www.vertiv.com/en-us/insights/articles/educational-articles/what-are-the-different-types-of-ups-systems/ | 廠商技術文章 | 中高 |
| S-03 | Schneider Electric：How double-conversion online UPS operate<br>https://www.se.com/th/en/faqs/FA383827 | 廠商 FAQ | 中高 |
| S-04 | CUNY Pressbooks：Why Do Industrial Systems Use 3 Phase Isolation Transformers?<br>https://pressbooks.cuny.edu/vision/part/why-industrial-systems-use-3-phase-isolation-transformers/<br>EEPower：Understanding the basics of isolation transformers<br>https://eepower.com/technical-articles/understanding-the-basics-of-isolation-transformers/<br>Delta-Wye Isolation Transformer Grounding: Single-Point Bond Guide<br>https://industrialmonitordirect.com/blogs/knowledgebase/delta-wye-isolation-transformer-grounding-single-point-bond-guide | 教材／技術文章 | 中 |
| S-05 | kVA 轉安培教學頁<br>https://www.ankersolix.com/blogs/ac-power/how-to-convert-kva-to-amps<br>https://hvacbase.org/3-phase-power-calculator | 教學頁 | 中低，只取公式並自行驗算 |
| S-06 | Wikipedia：IEC 60446<br>https://en.wikipedia.org/wiki/IEC_60446 | 百科 | 中高 |
| S-07 | QElectroTech 官網與 GitHub wiki<br>https://qelectrotech.org/<br>https://github.com/qelectrotech/qelectrotech-source-mirror/wiki/tips_and_tricks | 官方 | 高 |

## W-01 系統總覽（S-01）
供電路徑：大樓配電盤（3φ3W 220V）→ 30 kVA 隔離變壓器（Δ220V → Y380/220V）→ 30 kVA UPS → X1 插座（IEC 60309 32A）→ 掃描探針顯微鏡。
電池：YUASA 12V × 32 只，掛在 UPS 直流側。
接地：大樓接地與專用接地棒共同接到 PE 排，變壓器 X0 點另接。

## W-02 雙轉換（online）UPS（S-02、S-03）
- 持續把輸入交流轉成直流，再轉回交流，讓負載與電網擾動隔開。
- 整流器在 DC link 產生直流，逆變器再產生輸出交流；電池接在 DC link 上。
- 市電中斷時整流器退出，電池繼續供電，不需要轉換開關。
- 切換到電池的時間為零；line-interactive 型約有 4–6 毫秒的中斷。
- 市電恢復後，整流器重新承擔負載並對電池充電。

對應圖：UPS 方塊、電池箱。

## W-03 隔離變壓器 Δ-Y（S-04）
- Δ-Y 是工業隔離變壓器常用組合：Δ 初級讓諧波不傳回電源側，Y 次級提供中性線。
- Δ 繞組讓三倍次諧波在內部環流。
- 次級接地可降低初級到次級的共模雜訊耦合。
- 低頻電壓驟降仍可能通過隔離變壓器，所以後面還需要 UPS。
- 驗算：380 ÷ 1.732 ≈ 219 V，與 380/220V 吻合。

對應圖：30 kVA、Δ220V → Y380/220V、X0。

## W-04 接地與單點接合（S-04）
- 中性線與設備接地導體應只在一個點接合。
- 多處接合會讓中性線電流分流到接地導體，形成並聯回路。
- 次級單獨接地可避免地線電流經初級接地系統回流。

對應圖：變壓器 X0 接 PE 排。

## W-05 導線顏色（S-06、S-01）
- IEC 60446：中性線用藍色，綠／黃雙色專用於保護導體（PE）。
- 三相相線慣用 L1 棕、L2 黑、L3 灰。
- 美國、加拿大、日本的註記允許不同顏色。
- 本圖有兩套顏色：第 1 頁以電壓／電流類型著色（橘 220V、藍 380V、紅 DC、綠黃 PE）；第 2 頁用 IEC 導線色（L1 棕、L2 黑、L3 灰、N 藍、PE 綠黃）。所以藍色在第 1 頁是 380V，在第 2 頁是中性線。

## W-06 三相 kVA → 安培（S-05，已自行驗算）
- 線電流 = kVA × 1000 ÷ (1.732 × 線間電壓)，平衡負載的估計值。
- 1.732 來自三相幾何關係，不是功率因數修正。
- kW = kVA × 功率因數，所以 kW 不會大於 kVA。

用此公式重算 S-01 第 2 頁電流表，全部吻合：

| 負載 | 220V 側 | 380V 側 | 電池側（÷0.94÷32÷12V） |
|---|---|---|---|
| 21 kVA | 55.1 A | 31.9 A | 58.2 A |
| 7.8 kVA | 20.5 A | 11.9 A | 21.6 A |
| 1.1 kVA | 2.9 A | 1.7 A | 3.0 A |

## W-07 停電可撐時間估算（S-01）
S-01 的電池計算書：24 kW 可放電 60 分鐘。
公式：可撐時間 = 60 分鐘 × 24 kW ÷ 負載 kW。
24 kW → 60 分鐘；7.8 kW → 約 185 分鐘；1.1 kW → 約 1,309 分鐘（已重算）。
這個公式假設可用能量固定為 24 kW × 60 分鐘。

## W-08 用 QElectroTech 畫接線圖（S-07）
- 開源圖面軟體，專案以 XML 格式儲存，有 8,200 多個符號與自動編號。
- 一個專案是存在單一 .qet 檔的多張頁面（folio），從元件庫拖曳符號、用導線連接。
- 好習慣：一頁一個功能、由左到右、間距一致、少交叉。

對應圖：第 1 頁單線示意、第 2 頁端子接線、第 3 頁外觀尺寸。

## 簡報與 Wiki 對應

| 投影片 | 主題 | 依據頁面 |
|---|---|---|
| 1–2 | 標題、目標 | W-01 |
| 3 | 供電全貌圖 | W-01、W-04 |
| 4 | UPS 雙轉換 | W-02 |
| 5 | 隔離變壓器 | W-03 |
| 6 | 接地 | W-04 |
| 7 | 顏色兩套系統 | W-05 |
| 8 | 三相電流計算機 | W-06 |
| 9 | 停電可撐時間 | W-07 |
| 10 | 常見陷阱 | W-05、W-06 |
| 11 | 用 QElectroTech 畫 | W-08 |
| 12–13 | 小測驗、參考資料 | 全部 |
