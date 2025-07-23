找到ubuntu puppeteer的路径
const puppeteer = require('puppeteer');
console.log(puppeteer.executablePath());

ubuntu
/home/elvelyn/.cache/puppeteer/chrome/linux-138.0.7204.157/chrome-linux64/chrome --no-sandbox --user-data-dir=/home/elvelyn/egreter/

windows
C:\Users\earwe\.cache\puppeteer\chrome\win64-138.0.7204.157\chrome-win64\chrome.exe
"C:\Users\earwe\.cache\puppeteer\chrome\win64-138.0.7204.157\chrome-win64\chrome.exe"  --user-data-dir="D:\Dev\Devts\egreter"


google-chrome --disable-gpu --no-sandbox --user-data-dir=/home/elvelyn/egreter/
