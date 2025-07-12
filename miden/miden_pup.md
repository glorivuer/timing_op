使用Typescript编写Puppeteer脚本,分析一下这个Prompt ,并给出更好的Prompt建议并加入这些功能:
当前chrome网页有多个实例运行 ,其中有一个实例的网页的是已开启加载了本地的Miden Wallet扩展chrome-extension://ablmompanofnodfdkgchkpmphailefpb/fullpage.html#/receive
找到目标页面: 扩展chrome-extension://ablmompanofnodfdkgchkpmphailefpb/fullpage.html#/receive
执行点击: 一旦定位到页面，查找并点击包含所有的 "Claim" 按钮。Claim按钮元素是 <button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button>
Claim按钮元素列表是<div class="flex flex-col gap-y-4 p-6"><div class="flex justify-center"><div class="relative left-[-33%] md:left-[-19%]"><p class="text-md text-gray-100">Ready to claim</p></div></div><div class="flex justify-center items-center gap-8"><div class="flex items-center gap-x-2"><svg viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg" name="arrow-right-down-filled-circle" class="w-8 h-8"><rect width="32" height="32" rx="16" fill="#F3F3F3"></rect><path d="M15.7583 16.7001L11.987 12.9301L12.9297 11.9868L16.701 15.7581L20.0003 12.4581V20.0001H12.4583L15.7583 16.7001Z" fill="black"></path></svg><div class="flex flex-col"><p class="text-md font-bold">1000 MIDEN</p><p class="text-xs text-gray-100">0xb6f2b...2a4b</p></div></div><button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button></div><div class="flex justify-center items-center gap-8"><div class="flex items-center gap-x-2"><svg viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg" name="arrow-right-down-filled-circle" class="w-8 h-8"><rect width="32" height="32" rx="16" fill="#F3F3F3"></rect><path d="M15.7583 16.7001L11.987 12.9301L12.9297 11.9868L16.701 15.7581L20.0003 12.4581V20.0001H12.4583L15.7583 16.7001Z" fill="black"></path></svg><div class="flex flex-col"><p class="text-md font-bold">1000 MIDEN</p><p class="text-xs text-gray-100">0xb6f2b...2a4b</p></div></div><button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button></div><div class="flex justify-center items-center gap-8"><div class="flex items-center gap-x-2"><svg viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg" name="arrow-right-down-filled-circle" class="w-8 h-8"><rect width="32" height="32" rx="16" fill="#F3F3F3"></rect><path d="M15.7583 16.7001L11.987 12.9301L12.9297 11.9868L16.701 15.7581L20.0003 12.4581V20.0001H12.4583L15.7583 16.7001Z" fill="black"></path></svg><div class="flex flex-col"><p class="text-md font-bold">1000 MIDEN</p><p class="text-xs text-gray-100">0xb6f2b...2a4b</p></div></div><button class="bg-primary-500 focus:bg-primary-500 hover:bg-primary-600 flex justify-center items-center gap-x-2 py-3 px-4 rounded-lg transition duration-300 ease-in-out w-[75px] h-[36px] text-md" type="button"><span class="text-white font-medium text-base">Claim</span></button></div></div>
设置循环: 每分钟或每几十秒执行一次.
会使用tsx命令来执行这个脚本,不需要tsconfig.

//
打开 任务管理器 (快捷键 Ctrl + Shift + Esc)。
切换到 “详细信息” (Details) 标签页。
在列表中找到所有名为 chrome.exe 的进程。
逐个选中它们，然后点击“结束任务” (End task)，直到列表里再也看不到 chrome.exe。

在您方便的位置创建一个新文件夹，例如，我在 C 盘根目录创建了一个名为 MyChromeDevProfile 的文件夹。
路径是：C:\MyChromeDevProfile
第三步：使用新的命令启动 Chrome
现在，使用包含了 --user-data-dir 参数的新命令来启动 Chrome。
按 Win + R，打开“运行”对话框。
复制并粘贴以下命令，然后按回车：
Generated cmd
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="C:\MyChromeDevProfile"


注意:
--user-data-dir 参数指向您刚刚创建的那个新文件夹。
请确保您的 Chrome 安装路径是 "C:\Program Files\Google\Chrome\Application\chrome.exe"，如果不是，请修改为您的实际路径。
执行后，您会看到一个全新的、像第一次安装一样干净的 Chrome 浏览器窗口被打开。它不会有任何书签、扩展或登录信息，也不会跳出账号选择界面。
第四步：验证远程调试是否成功
现在，在这个新打开的 Chrome 浏览器中，或者在您系统的任何其他浏览器中，访问：
http://127.0.0.1:9222/json/version
如果一切顺利，您将不再看到“无法连接”的错误，而是会看到一个包含 webSocketDebuggerUrl 的 JSON 页面。这表示远程调试已成功开启！
第五步：在新浏览器中安装 Miden Wallet 扩展
因为这是一个全新的浏览器环境，您需要在这个窗口中：
访问 Chrome 网上应用店。
搜索并安装 Miden Wallet 扩展。
完成扩展的初始设置，并打开目标页面 chrome-extension://.../fullpage.html#/receive。
第六步：运行您的 Puppeteer 脚本
现在，将 webSocketDebuggerUrl 的值复制到您的 TypeScript 脚本中，然后运行 tsx claim_miden.ts，脚本就能成功连接并执行任务了。

//
如果您坚持要使用您现有的某个个人资料，可以这样做：
在您日常使用的 Chrome 中访问 chrome://version。
找到 “个人资料路径” (Profile Path) 这一项，并复制它的值。注意：要复制的是上一级目录，例如，如果路径是 C:\Users\YourUser\AppData\Local\Google\Chrome\User Data\Profile 1，您需要的是 C:\Users\YourUser\AppData\Local\Google\Chrome\User Data。
完全关闭所有 Chrome 进程 (参考第一步)。
使用以下命令启动：
Generated cmd
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="C:\Users\YourUser\AppData\Local\Google\Chrome\User Data"
Use code with caution.
Cmd
这种方法风险在于，如果您的主浏览器已经在运行，可能会导致冲突。所以，强烈推荐使用第一种方案。


C:\Users\chou\AppData\Local\Google\Chrome\User Data\Profile 30

"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="C:\Users\chou\AppData\Local\Google\Chrome\User Data"

//

{
   "Browser": "Chrome/138.0.7204.101",
   "Protocol-Version": "1.3",
   "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36",
   "V8-Version": "13.8.500258",
   "WebKit-Version": "537.36 (@a2159ad9ab8037bd34a21b9903003e0964c2075b)",
   "webSocketDebuggerUrl": "ws://127.0.0.1:9222/devtools/browser/cfb79266-0d3b-4ef2-8050-a639af8e0340"
