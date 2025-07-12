在 Linux 上:
Generated bash
google-chrome  --disable-gpu --remote-debugging-port=9222

/home/elvelyn/.config/google-chrome/Default/

google-chrome --disable-gpu --user-data-dir=/home/elvelyn/Mychrome/ --remote-debugging-port=9222


启动后，在浏览器中访问 http://127.0.0.1:9222/json/version。您会看到一个 JSON 输出，其中包含一个 webSocketDebuggerUrl 字段。复制这个 URL，它就是我们脚本需要的连接地址。


windows

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
