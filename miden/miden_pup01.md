使用Typescript编写Puppeteer脚本,脚本会使用tsx命令执行,分析一下这个Prompt ,并给出更好的Prompt建议主要功能点如下,下列每一步骤都要有console log来显示成功或错误.
1 开启指定路径的chome,executablePath ,C:\Program Files\Google\Chrome\Application,userDataDir用户数据目录路径是:D:\Devts\Miden01\Elven,设置 headless: false.
2 去到网站https://faucet.testnet.miden.io/ , 简称A页签,找到元素input id="account-address", 录入0x501c2385d304ca10000849fc6e1855,选择<select id="asset-amount">,<option value="1000">1000</option>
3 浏览器新增页签chrome-extension://ablmompanofnodfdkgchkpmphailefpb/fullpage.html，简称B页签,在页签元素 id="unlock-password" type="password"，输入密码Glor-969696，点击<button>解锁</button>,点击<span>接收</span>.
4 浏览器到A页签，点击button id="button-public",内容是Send Public Note
5 转到B页签,等待5秒,点击<span>Claim</span>，等待2秒，如没有找到Claim按键，刷新页面后等待5秒，点击<span>Claim</span>，如刷新页面等待5秒后没有找到Claim按键,终止程序.
6 循环4和5步骤.




2 点击当前A页签的#button-private按钮，会自动下载一个note.mno的文件在目录C:\Users\chou\Downloads下
3 等待10秒
4 点到B页签，点击Upload file 按钮上传刚才下载路径的文件.

<span class="text-black font-medium text-base">Upload file</span>

document.querySelectorAll('#Upload file')

document.querySelector("span").textContent = "Upload file"

document.querySelector('span')

document.querySelectorAll("span").textContent = "Upload file"

document.querySelectorAll('button.w-5\\/6[type="button"]')
