好的，您提出的这个Chrome插件 "Replicate Operator" 的想法非常棒，功能点描述得相当清晰和完整。这已经是一个非常高质量的Prompt了。

为了让这个Prompt更具可执行性，并且覆盖一些在实际开发中可能遇到的细节问题，我将基于您的描述，提供一个经过优化和补充的、更完善的Prompt。这可以帮助开发者更精确地理解需求，并预见到一些技术挑战。

---

### 优化后的开发Prompt：Replicate Operator

**插件名称:** Replicate Operator

**核心定位:** 一个类似Chrome DevTools Recorder的用户网页操作录制与回放工具，通过侧边栏（Side Panel）提供简洁直观的用户界面。

---

### 一、核心功能模块 (Core Functional Modules)

#### **1. UI与交互 (UI & Interaction)**

*   **1.1. 激活方式:** 点击浏览器工具栏的 "Replicate Operator" 插件图标，在当前标签页右侧打开一个侧边栏（Side Panel）。
*   **1.2. 初始界面:** Side Panel 默认显示：
    *   一个醒目的 **"Start Recording"** 按钮。
    *   一个用于显示已保存操作流的列表区域（初始为空）。
    *   列表区域下方的操作按钮（"Run Flow", "Display Flow", "Delete"），在未选中任何操作流时为禁用（disabled）状态。
*   **1.3. 录制中界面:** 点击 "Start Recording" 后，界面更新为：
    *   **"Finish & Save"** 按钮。
    *   **"Cancel"** 按钮。
    *   一个名为 **"Live Recording Steps"** 的实时步骤显示区域。

#### **2. 录制逻辑 (Recording Logic)**

*   **2.1. 启动录制:** 点击 "Start Recording" 后，插件立即开始监听并记录当前活动标签页的用户操作。
*   **2.2. 录制范围:**
    *   **仅记录**当前主网页文档中的操作，完全忽略在Side Panel内部的任何交互。
    *   **初始导航:** 录制开始时的第一个步骤自动记录为 `goto` 操作，值为当前页面的 `location.href`。
    *   **用户操作:**
        *   **点击 (Click):** 监听 `click` 事件。
        *   **输入/变更 (Change/Input):** 监听 `<input>`, `<textarea>`, `<select>` 等表单元素的 `change` 事件，记录其最终值。为避免记录每一个按键，不使用 `keyup` 或 `keydown`，只在元素失去焦点或发生 `change` 事件时记录一次。
        *   **删除 (Deletion):** 对于输入框内容的删除，同样通过 `change` 事件记录最终的空值或修改后的值。
*   **2.3. 元素选择器策略 (Selector Strategy):**
    *   **核心要求:** **严禁使用**基于坐标 (x, y) 的定位。
    *   **优先策略:** 自动生成最稳定、最唯一的CSS选择器。优先级如下：
        1.  拥有唯一 `id` 的元素 (e.g., `#submit-button`)。
        2.  拥有特殊且唯一性的属性 (e.g., `[data-testid="..."]`, `[name="..."]`)。
        3.  简洁的类名组合 (e.g., `.btn.btn-primary`)。
        4.  作为最后的备选方案，使用结构化路径 (e.g., `div > form > button:nth-of-type(1)`)。
*   **2.4. 等待时间 (Wait Times):**
    *   精确记录两个连续用户操作之间的**静默等待时间**（以毫秒为单位）。例如，用户点击按钮A，等待2.5秒，然后开始在输入框B中输入，那么就需要记录一个 `waitFor: 2500` 的步骤。

#### **3. 实时显示与步骤定义 (Live Display & Step Definition)**

*   **3.1. 实时更新:** "Live Recording Steps" 区域应在每次捕获到有效操作后立即更新，向用户实时反馈录制情况。
*   **3.2. 步骤格式化:** 每个步骤都应清晰地展示。建议的步骤数据结构如下：
    ```json
    { "type": "goto", "url": "https://example.com" },
    { "type": "wait", "duration": 1500 },
    { "type": "click", "selector": "#login-button", "label": "Login" },
    { "type": "wait", "duration": 800 },
    { "type": "change", "selector": "input[name='username']", "value": "my_user" }
    ```
*   **3.3. 步骤分组逻辑 (替代原第4点):** 原Prompt的步骤分组逻辑（“如没有Click...”）在实际中可能不清晰。建议采用更明确的事件驱动模型：
    *   每一次 `goto`, `click`, `change` 都被视为一个核心操作步骤。
    *   在两个核心操作步骤之间的时间差，记录为一个 `wait` 步骤。
    *   这样可以更精确地回放用户的操作节奏。

#### **4. 保存与取消 (Save & Cancel)**

*   **4.1. 完成保存:**
    *   点击 **"Finish & Save"** 按钮，弹出一个简单的模态框（modal/prompt）要求输入名称。
    *   默认名称格式为 `OperateFlow-YYYYMMDDHHmmss` (e.g., `OperateFlow-20231027153000`)。
    *   用户可以修改名称。保存时必须进行**重名检查**，如果名称已存在，需提示用户更换。
    *   操作流数据保存在 `chrome.storage.local` 中，以便持久化。
    *   保存成功后，清空 "Live Recording Steps" 区域，并将新的操作流名称添加到底部的列表中。
*   **4.2. 取消操作:**
    *   点击 **"Cancel"** 按钮，立即丢弃当前所有已录制的步骤，并清空 "Live Recording Steps" 区域。
*   **4.3. 意外中断处理:**
    *   如果用户在录制过程中关闭了标签页或浏览器，当前未保存的录制数据将**自动丢失**。当用户再次打开插件时，界面应恢复到初始状态，不保留任何未保存的记录。这可以通过将实时录制数据存储在Side Panel的运行时内存（非持久化变量）中来实现。

#### **5. 操作流管理与执行 (Flow Management & Execution)**

*   **5.1. 列表显示:** 所有已保存的操作流都显示在 "Live Recording Steps" 下方的列表中，每个名称前带有一个**单选按钮 (radio button)**，确保一次只能选中一个。
*   **5.2. 执行流程 (Run Flow):**
    *   选中一个已保存的操作流。
    *   点击 **"Run Flow"** 按钮。
    *   插件将**自动打开一个新的Chrome标签页**。
    *   在新标签页中，严格按照保存的步骤（包括`goto`, `wait`, `click`, `change`等）顺序执行。
    *   **错误处理:** 如果在执行过程中，某个元素的CSS选择器未能在页面上找到，执行应立即停止，并在Side Panel中向用户报告错误，指明失败的步骤和原因。
*   **5.3. 显示流程 (Display Flow):**
    *   选中一个已保存的操作流。
    *   点击 **"Display Flow"** 按钮。
    *   插件打开一个新的标签页，页面内容以**格式化、易于阅读**的方式（例如使用表格或代码块）展示该操作流的所有步骤、选择器、值和等待时间。
*   **5.4. 删除流程 (Delete):**
    *   选中一个已保存的操作流。
    *   点击 **"Delete"** 按钮。
    *   弹出确认对话框（"Are you sure you want to delete this flow?"）。
    *   确认后，从 `chrome.storage.local` 和界面列表中永久删除该操作流。

---

### 二、技术架构建议 (Technical Architecture Suggestions)

*   **`manifest.json`:**
    *   `"permissions": ["sidePanel", "storage", "tabs", "scripting", "activeTab"]`
    *   `"host_permissions": ["<all_urls>"]` (允许内容脚本在所有页面运行)
    *   `"background": { "service_worker": "background.js" }`
    *   `"side_panel": { "default_path": "sidepanel.html" }`
*   **Content Script (`content_script.js`):**
    *   负责注入到目标网页中，监听DOM事件（`click`, `change`）。
    *   负责生成稳定的CSS选择器。
    *   将捕获到的操作事件和数据发送给Background Script或Side Panel。
*   **Background Script (`background.js`):**
    *   作为中央协调器，管理插件的状态（如：是否正在录制）。
    *   处理来自Content Script和Side Panel的消息。
    *   负责所有与 `chrome.storage.local` 的交互（保存、加载、删除）。
    *   负责执行 "Run Flow" 的逻辑，包括创建新标签页和通过 `chrome.scripting.executeScript` 注入执行脚本。
*   **Side Panel (`sidepanel.html` / `sidepanel.js`):**
    *   构建用户界面。
    *   管理UI状态的切换（例如，按钮的显隐和禁用状态）。
    *   向Background Script发送指令（如 "start recording", "save flow"）。
    *   从Background Script接收数据并更新UI（如 "Live Recording Steps" 和已保存列表）。
*   **Replay Script (`replay_script.js`):**
    *   一个专门用于回放的脚本，由Background Script在执行"Run Flow"时注入到新页面。
    *   接收操作步骤数组，并按顺序模拟用户操作（`element.click()`, `element.value = ...`, `dispatchEvent(...)`等）。

---

这个优化后的Prompt不仅定义了“做什么”，还提供了“怎么做”的建议，使需求更加明确、具体，并考虑了健壮性和用户体验，可以作为一份非常出色的开发需求文档。
