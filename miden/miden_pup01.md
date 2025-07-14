使用Typescript编写Puppeteer脚本,分析一下这个Prompt ,并给出更好的Prompt建议并加入这些功能: 主要功能点如下,
1 开启路径的chome,并在某个profile 
2 去到网站faucet
3 点击chrome extension , 输入密码，点击最大化按钮，点击接收按钮 ，点击复制地址，
4 到前一个页签，黏贴地址 ，点击public or private
5 点击到miden 页签，找到button ,点击.  5秒后 查找 ，如果有 ，再等5秒，2次等待还有，刷新页面，如果还有，报错停止程序
如果没有，转到第一页签
6  循环处理

2 点击当前A页签的#button-private按钮，会自动下载一个note.mno的文件在目录C:\Users\chou\Downloads下
3 等待10秒
4 点到B页签，点击Upload file 按钮上传刚才下载路径的文件.

<span class="text-black font-medium text-base">Upload file</span>

document.querySelectorAll('#Upload file')

document.querySelector("span").textContent = "Upload file"

document.querySelector('span')

document.querySelectorAll("span").textContent = "Upload file"

document.querySelectorAll('button.w-5\\/6[type="button"]')
