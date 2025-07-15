使用Typescript编写Puppeteer脚本,脚本会使用tsx命令执行,分析一下这个Prompt ,并给出更好的Prompt建议主要功能点如下,下列每一步骤都要有console log来显示成功或错误.
1 开启指定路径的chome和profile
2 去到网站https://faucet.testnet.miden.io/ , 简称A页签
3 点击已经固定在浏览器头部上chrome extension的Miden Wallet ,出现悬浮窗口, 
在悬浮窗口找到元素 id="unlock-password" type="password"，输入密码Glor-969696，点击<button>解锁</button>,点击<title id="maximiseIconTitle">Maximise View</title>，chrome浏览器会打开第二页签Miden Wallet，简称B页签.
4 点击<span>接收</span>,点击<svg name="copy"></svg>复制地址.
5 转到A页签，找到元素input id="account-address", 黏贴地址 .
6 点击button id="button-public",内容是Send Public Note
7 等待5秒
8 转到B页签




2 点击当前A页签的#button-private按钮，会自动下载一个note.mno的文件在目录C:\Users\chou\Downloads下
3 等待10秒
4 点到B页签，点击Upload file 按钮上传刚才下载路径的文件.

<span class="text-black font-medium text-base">Upload file</span>

document.querySelectorAll('#Upload file')

document.querySelector("span").textContent = "Upload file"

document.querySelector('span')

document.querySelectorAll("span").textContent = "Upload file"

document.querySelectorAll('button.w-5\\/6[type="button"]')
