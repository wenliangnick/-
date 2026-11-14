# AGENTS.md - 大學統計學：機率分布筆記網站規範

## 1. 專案目的 (Project Purpose)
本專案是一個模組化、可持續擴充的「大學數理統計與機率分布筆記」靜態網站，託管於 GitHub Pages。
核心目標是讓開發者只需提供分布名稱或基礎筆記，AI 就能嚴格遵循本規範，快速生成版面一致、數學符號嚴謹、支援 LaTeX 渲染的獨立網頁，並自動更新首頁索引。

## 2. 技術棧與整體風格 (Tech Stack & Design Style)
- **技術限制**：純原生 HTML5、CSS3、Vanilla JavaScript，**禁止引入 React、Vue 或需要打包編譯的框架**，確保推送至 GitHub Pages 後可直接瀏覽。
- **數學公式渲染**：全站統一使用 **MathJax 3**（透過 CDN 載入），行內公式使用 `$ ... $`，獨立區塊公式使用 `$$ ... $$`。
- **視覺風格**：
  - 學術筆記極簡風（Academic Minimalism），高對比、易閱讀。
  - 主色調：深海藍（`#1e3a8a`）代表離散型分布，森林綠（`#065f46`）代表連續型分布，背景為柔和米白/淺灰（`#f8fafc`）。
  - 所有樣式必須寫在 `/assets/css/style.css` 中，**禁止在 HTML 檔案內寫 `<style>` 標籤或大量 inline style**。

## 3. 檔案命名與放置規則 (File Naming & Structure)
- 所有分布頁面必須依照性質分類存放：
  - 離散型分布：`/distributions/discrete/<english-name>.html`
  - 連續型分布：`/distributions/continuous/<english-name>.html`
- **檔名規範**：一律使用全小寫英文，單字間以連字號 `-` 分隔，不加 `distribution` 字尾。
  - 正確範例：`normal.html`、`binomial.html`、`chi-square.html`、`student-t.html`、`gamma.html`
  - 錯誤範例：`NormalDistribution.html`、`chi_square.html`

## 4. 單一分布頁面標準結構 (Page Template & Content Format)
新增任何一個分布頁面時，`<main class="note-container">` 內部**必須嚴格包含以下 7 個區塊**，順序不可調動：

1. **Header 標題區**：
   - 中文名稱、英文名稱、分類標籤（離散型 / 連續型）與標準記號（例如：$X \sim N(\mu, \sigma^2)$）。
2. **直觀意義與應用場景 (Intuition & Use Cases)**：
   - 用白話解釋這個分布「在算什麼」、常見的實際生活或統計檢定場景。
3. **核心定義與函數 (Definition & Functions)**：
   - 參數空間（Parameter Space）與支撐集（Support，$x$ 的範圍）。
   - 機率質量函數（PMF）或機率密度函數（PDF）。
   - 累積分布函數（CDF，若無封閉解請明確註明）。
4. **核心統計量摘要表 (Key Statistics Table)**：
   - 使用標準 `<table class="stats-table">` 呈現：**期望值 $E[X]$**、**變異數 $\text{Var}(X)$**、**動差母函數 (MGF) $M_X(t)$**、**特徵函數 $\varphi_X(t)$**。
5. **關鍵性質與定理 (Key Properties & Theorems)**：
   - 例如：可加性（Additivity）、無記憶性（Memorylessness）、與其他分布的極限或轉換關係（如當 $n \to \infty$ 時的漸近行為）。
6. **重點推導過程 (Step-by-Step Derivation)**：
   - 至少包含一項核心推導（例如：如何從定義推導出 $E[X]$、$\text{Var}(X)$ 或 MGF），使用 `<div class="derivation-box">` 包裝，步驟必須完整跳步不可過大。
7. **頁面底部導覽 (Footer Navigation)**：
   - 提供「返回首頁目錄」與「上一個／下一個分布」的連結。

## 5. AI 執行新增與修改的強制規則 (AI Rules)
1. **同步更新索引**：每當在 `/distributions/` 下新增一個 HTML 檔案後，**必須同步更新 `index.html`** 中的分布列表卡片（包含名稱、記號、分類標籤與正確的相對路徑）。
2. **符號一致性**：全站統一使用 $E[X]$ 表示期望值、$\text{Var}(X)$ 表示變異數、$f_X(x)$ 表示 PDF、$p_X(x)$ 表示 PMF、$F_X(x)$ 表示 CDF。
3. **不破壞共用元件**：修改單一筆記頁面時，不得擅自更改 `style.css` 的全域變數或 `main.js` 的核心邏輯，除非明確收到修改全站樣式的指令。
4. **資料來源整合**：若使用者提供 Wiki 內容或零散課堂筆記，AI 需自動將內容拆解並填入上述 7 個標準區塊中；若有缺少的統計量（如 MGF），AI 應主動補齊並標註。