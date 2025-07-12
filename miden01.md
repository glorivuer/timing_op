开发一个chrome extension名称叫做Miden Click, 主要功能点如下,分析一下这个Prompt ,并给出更好的Prompt建议并加入这些功能:
1 插件点击后在网页上出现Side Panel,上面有Start Operating按钮和 Finish Operating按钮.
2 当前网页有个页签已开启，页签是另外一个chrome extension的页签, chrome-extension://ablmompanofnodfdkgchkpmphailefpb/fullpage.html#/receive
3 Start Operating按钮后，每分钟查询一下这个页面上面有没有新的claim按钮出现.每分钟这个时间可修改.按钮元素是
<button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button>
4 claim按钮是列表多个出现.例子如下:<div class="flex flex-col gap-y-4 p-6"><div class="flex justify-center"><div class="relative left-[-33%] md:left-[-19%]"><p class="text-md text-gray-100">Ready to claim</p></div></div><div class="flex justify-center items-center gap-8"><div class="flex items-center gap-x-2"><svg viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg" name="arrow-right-down-filled-circle" class="w-8 h-8"><rect width="32" height="32" rx="16" fill="#F3F3F3"></rect><path d="M15.7583 16.7001L11.987 12.9301L12.9297 11.9868L16.701 15.7581L20.0003 12.4581V20.0001H12.4583L15.7583 16.7001Z" fill="black"></path></svg><div class="flex flex-col"><p class="text-md font-bold">1000 MIDEN</p><p class="text-xs text-gray-100">0xb6f2b...2a4b</p></div></div><button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button></div><div class="flex justify-center items-center gap-8"><div class="flex items-center gap-x-2"><svg viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg" name="arrow-right-down-filled-circle" class="w-8 h-8"><rect width="32" height="32" rx="16" fill="#F3F3F3"></rect><path d="M15.7583 16.7001L11.987 12.9301L12.9297 11.9868L16.701 15.7581L20.0003 12.4581V20.0001H12.4583L15.7583 16.7001Z" fill="black"></path></svg><div class="flex flex-col"><p class="text-md font-bold">1000 MIDEN</p><p class="text-xs text-gray-100">0xb6f2b...2a4b</p></div></div><button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button></div><div class="flex justify-center items-center gap-8"><div class="flex items-center gap-x-2"><svg viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg" name="arrow-right-down-filled-circle" class="w-8 h-8"><rect width="32" height="32" rx="16" fill="#F3F3F3"></rect><path d="M15.7583 16.7001L11.987 12.9301L12.9297 11.9868L16.701 15.7581L20.0003 12.4581V20.0001H12.4583L15.7583 16.7001Z" fill="black"></path></svg><div class="flex flex-col"><p class="text-md font-bold">1000 MIDEN</p><p class="text-xs text-gray-100">0xb6f2b...2a4b</p></div></div><button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button></div></div>
5 如有可点击的claim按钮，轮流点击这些claim按钮.
6 点击Finish Operating按钮终止这个循环.

好的，我们来详细分析一下您提供的这个Prompt，并基于此提出一个更全面、更健壮、对开发者更友好的优化版本。

---

### 对现有Prompt的分析

这是一个非常好的起点。它清晰地表达了核心需求，对于一个初步的想法来说，信息量已经很足了。

**优点:**

1.  **目标明确:** 核心功能“自动点击Claim按钮”非常清晰。
2.  **名称已定:** `Miden Click`，简洁明了。
3.  **关键UI元素已定义:** Side Panel、Start/Finish按钮。
4.  **目标页面精确:** 提供了完整的URL `chrome-extension://...`，这对开发者来说是至关重要的信息，避免了猜测。
5.  **目标元素具体:** 提供了按钮的完整HTML代码和其所在的列表结构，这是最有价值的部分，大大降低了开发的实现难度。

**可以改进和补充的“问题点” (从开发者角度看):**

1.  **用户反馈缺失:** 当用户点击"Start"后，插件开始工作，但用户如何知道它正在运行？它找到了多少个按钮？是否成功点击了？如果没找到目标页面会怎样？当前的UI设计没有提供任何状态反馈。
2.  **鲁棒性考虑不足 (Edge Cases):**
    *   如果目标`chrome-extension://`页签没有打开怎么办？
    *   如果点击按钮后，页面需要几秒钟才响应（例如，出现一个加载动画），快速连续点击可能会导致问题。
    *   如果点击某个按钮后，它本身就从页面上消失了，下一次循环是否会重复计数或尝试点击一个不存在的元素？
    *   如果网络错误或页面崩溃，循环应该如何处理？
3.  **配置灵活性:** 只提到了“每分钟这个时间可修改”，但没有说明用户如何修改。是硬编码在代码里，还是应该在UI上提供一个输入框？
4.  **交互细节模糊:** “轮流点击这些claim按钮” 是指一次性把所有找到的按钮都点完，还是每分钟只点击一个？点击之间是否需要间隔？（快速连续点击很容易被网站识别为机器人行为）。
5.  **技术可行性提示:** 与普通的`https://`网页不同，一个Chrome插件默认是无法直接操作另一个插件的页面的（`chrome-extension://`协议）。这需要特殊的权限声明（`management`权限），并且实现起来比常规的网页自动化要复杂。Prompt中没有提及这一点，可能会给不熟悉的开发者带来预期外的挑战。

---

### 更好的Prompt建议

为了让这个插件更实用、更健壮、用户体验更好，我们可以把需求细化成一个更专业的开发需求文档。

**项目名称:** Miden Click

**项目目标:** 开发一款Chrome扩展程序，用于自动检测并点击在指定Miden扩展页面上出现的“Claim”按钮，以帮助用户自动收取奖励，并提供清晰的操作界面和反馈。

---

#### **一、核心功能 (User Stories)**

*   **作为用户,** 我希望能在任何页面打开一个侧边栏来控制自动化任务的开始和停止。
*   **作为用户,** 我希望能自定义检查“Claim”按钮的频率，以平衡及时性和系统资源的占用。
*   **作为用户,** 我希望当自动化任务运行时，能清晰地看到当前的状态（如：正在运行、已停止、正在查找目标页面等）。
*   **作为用户,** 我希望插件能够智能地找到目标页面，如果找不到，能给出明确的提示。
*   **作为用户,** 我希望能看到操作日志，了解插件在什么时间发现了多少按钮，并执行了点击操作。
*   **作为用户,** 我希望插件的点击行为能模拟人类，在每次点击之间有短暂的延迟，避免被目标应用识别为机器人。

---

#### **二、功能需求细化 (Functional Requirements)**

**1. 侧边栏界面 (Side Panel UI)**

*   **面板触发:** 点击Chrome工具栏的“Miden Click”图标，在当前页面右侧滑出Side Panel。
*   **状态显示:** 面板顶部应有一个状态指示器。
    *   `Status: Idle / Running / Stopped / Error / Target tab not found`
*   **控制按钮:**
    *   `Start Operating` 按钮: 开始执行任务。当任务运行时，此按钮应变为 `Pause Operating` 或禁用状态。
    *   `Finish Operating` 按钮: 彻底停止任务并重置状态。
*   **配置选项:**
    *   `Check Interval (seconds):` 一个输入框，允许用户输入数字，默认值为 `60`。
    *   `Click Delay (ms):` 一个输入框，用于设置在连续点击多个按钮时的间隔时间，默认值为 `2000` (即2秒)。
*   **活动日志 (Activity Log):**
    *   一个可滚动的文本区域，用于显示插件的操作历史。
    *   示例日志:
        *   `[14:30:01] Operation Started. Checking every 60 seconds.`
        *   `[14:31:02] Searching for target tab... Tab found.`
        *   `[14:31:03] Found 3 'Claim' buttons.`
        *   `[14:31:03] Clicking button 1... Success.`
        *   `[14:31:05] Clicking button 2... Success.`
        *   `[14:31:07] Clicking button 3... Success.`
        *   `[14:32:03] No new 'Claim' buttons found.`
        *   `[14:35:00] Error: Target tab was closed. Operation stopped.`
        *   `[14:40:10] Operation Finished by user.`

**2. 核心操作逻辑 (Core Logic)**

*   **开始操作 (Start Operating):**
    1.  读取用户设置的`Check Interval`和`Click Delay`值。
    2.  更新UI状态为 `Running`。
    3.  立即启动第一次检查，然后根据`Check Interval`设定的时间启动一个定时循环（使用`setTimeout`链而不是`setInterval`以避免任务重叠）。
*   **结束操作 (Finish Operating):**
    1.  清除所有定时器。
    2.  更新UI状态为 `Stopped`。
    3.  在日志中记录操作已停止。
*   **循环任务 (The Loop):**
    1.  **查找目标Tab:** 在所有打开的标签页中，精确查找URL为 `chrome-extension://ablmompanofnodfdkgchkpmphailefpb/fullpage.html#/receive` 的页面。
        *   **如果找到:** 继续下一步。
        *   **如果未找到:** 更新UI状态为 `Target tab not found`，在日志中记录，并暂停当前循环，等待下一次检查。
    2.  **注入脚本并查找按钮:**
        *   向目标Tab注入一个内容脚本 (Content Script)。
        *   该脚本负责查找页面上所有符合条件的`Claim`按钮。**精确的查找逻辑:** 查找所有`<button>`元素，其class列表包含 `bg-primary-500` 并且其内部的`<span>`文本内容为 `Claim`。同时，应只选择未被禁用的按钮 (`:not(:disabled)`)。
    3.  **执行点击:**
        *   如果找到了一个或多个按钮，在日志中记录找到的数量。
        *   **依次点击 (Sequentially):** 使用 `for...of` 循环和 `await` 结合一个延时函数，按顺序点击每个按钮。
        *   在每次点击前，等待`Click Delay`设定的毫秒数。
        *   在日志中记录每一次点击的尝试和结果。
    4.  **调度下一次运行:** 当前轮次（包括所有点击）完成后，设置一个`setTimeout`，在`Check Interval`秒后再次执行循环任务。

**3. 技术规格与权限 (Technical Specifications)**

*   **Manifest V3:** 插件应使用Manifest V3标准开发。
*   **权限 (permissions):**
    *   `sidePanel`: 用于实现侧边栏界面。
    *   `tabs`: 用于查找指定URL的标签页。
    *   `scripting`: 用于向目标页面动态注入脚本。
    *   `storage`: 用于保存用户的设置（如Interval和Delay），以便下次打开时保留。
    *   **`management`**: **[关键]** 此权限为必须，用于获取其他已安装扩展的信息，并可能需要它来与另一个扩展的页面进行交互。需要告知用户为何需要此敏感权限。

---

### 总结

这个优化后的Prompt，将一个简单的想法变成了一个结构清晰、考虑周全的需求文档。它不仅告诉开发者“做什么”，还解释了“为什么这样做”（通过User Stories），并提供了处理各种异常情况的指导，最终能开发出一款更稳定、更好用的工具。
