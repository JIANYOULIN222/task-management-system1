# 系統架構文件 (Architecture Document)

## 1. 系統總覽 (System Overview)
本任務管理系統 (Task Management System) 採用純前端 (Client-Side) 架構設計，不需要後端伺服器與外部資料庫。系統將直接在使用者的瀏覽器上執行，以確保最快的反應速度及最簡單的部署方式。

## 2. 技術選型 (Technology Stack)
- **結構與內容 (Structure)**: HTML5
- **樣式與版面 (Styling)**: CSS3 (純 CSS，不依賴外部框架如 Bootstrap 或 Tailwind)，採用 CSS Variables 進行主題與變數管理。
- **邏輯控制 (Logic)**: Vanilla JavaScript (ES6+)，不依賴 React、Vue 等前端框架，維持輕量與高效。
- **資料儲存 (Storage)**: Web Storage API (`localStorage`)。

## 3. 架構設計 (Architecture Design)

系統主要分為三個邏輯層：

### 3.1 呈現層 (Presentation Layer)
負責使用者介面的展示與互動。
- **`index.html`**: 定義應用程式的骨架，包含：
  - 輸入區域 (Task Input Form)
  - 任務列表容器 (Task List Container)
  - 狀態篩選器 (Status Filters)
- **`style.css`**: 實作現代化的 UI/UX 設計，包含玻璃擬物化 (Glassmorphism) 特效、平滑轉場動畫 (Transitions)、響應式設計 (Media Queries)。

### 3.2 業務邏輯層 (Business Logic Layer)
負責處理應用程式的核心邏輯與狀態管理。
- **`app.js`**:
  - **事件監聽 (Event Listeners)**: 捕捉使用者的輸入、點擊、刪除與篩選動作。
  - **狀態管理 (State Management)**: 維護一個包含任務物件的陣列 (Array of Task Objects)。每個任務物件包含 `id`, `text`, 和 `completed` 屬性。
  - **DOM 操作 (DOM Manipulation)**: 根據當前的狀態與篩選條件，動態生成與更新 HTML 元素，並渲染至畫面上。

### 3.3 資料持久層 (Data Persistence Layer)
負責將資料安全地儲存在使用者的設備上。
- **LocalStorage API**:
  - 當狀態發生改變（新增、完成、刪除）時，序列化 (JSON.stringify) 任務陣列並儲存至 `localStorage`。
  - 當應用程式初始化（網頁載入）時，從 `localStorage` 反序列化 (JSON.parse) 讀取資料，並恢復先前的狀態。

## 4. 資料模型 (Data Model)

任務 (Task) 的基本資料結構如下：

```javascript
{
  id: "1682859381234",  // String: 唯一識別碼 (通常使用 Date.now() 生成)
  text: "完成系統架構文件", // String: 任務內容
  completed: false      // Boolean: 完成狀態 (true = 已完成, false = 進行中)
}
```

## 5. 系統互動流程 (System Flow)

1. **初始化**: 網頁載入 $\rightarrow$ 讀取 `localStorage` $\rightarrow$ 渲染任務列表 $\rightarrow$ 綁定事件監聽器。
2. **新增任務**: 使用者輸入 $\rightarrow$ 觸發 Submit $\rightarrow$ 產生新 Task 物件 $\rightarrow$ 更新 State $\rightarrow$ 儲存至 `localStorage` $\rightarrow$ 重新渲染清單。
3. **更改狀態**: 點擊核取方塊 $\rightarrow$ 找到對應 Task ID $\rightarrow$ 切換 `completed` 值 $\rightarrow$ 更新 State $\rightarrow$ 儲存至 `localStorage` $\rightarrow$ 重新渲染或套用 CSS 樣式。
4. **刪除任務**: 點擊刪除按鈕 $\rightarrow$ 根據 Task ID 從陣列移除 $\rightarrow$ 更新 State $\rightarrow$ 儲存至 `localStorage` $\rightarrow$ 重新渲染清單。
5. **篩選任務**: 點擊篩選頁籤 $\rightarrow$ 更改目前 Filter 狀態 $\rightarrow$ 根據 Filter 狀態過濾 Array $\rightarrow$ 重新渲染清單。
