 const potentialHosts = document.querySelectorAll('.mx-auto.pb-2'); 

可能性 1 (最高概率): Shadow Host 定位错误
虽然 .mx-auto.pb-2 这个选择器在页面上能找到一个元素，但它可能不是我们期望的那个。
场景：一个网页上可能有很多个元素都带有 .mx-auto 和 .pb-2 这两个用于布局的 CSS 类。我们的脚本可能找到了第一个匹配的元素，但那个元素恰好不是包含 #shadow-root 的那一个。
验证方法：

打开你的目标网页和开发者工具。
在 "Console" (控制台) 面板中，输入 document.querySelectorAll('.mx-auto.pb-2') 然后按回车。

查看返回的 NodeList。如果 length 大于 1，就说明页面上有多个匹配的元素，我们的脚本很可能定位到了错误的那一个。


可能性 2 (中等概率): Iframe 是异步加载的
场景：脚本找到了正确的 Shadow Host，但就在那一瞬间，iframe 还没有被 JavaScript 完全渲染到 Shadow DOM 中。我们的 page.evaluateHandle 执行得太快了。
验证方法：这种情况很难直接验证，但我们可以通过在代码中加入等待来解决。
可能性 3 (较低概率): title 属性有细微差别
场景：title 属性可能包含了你看不到的特殊字符、额外的空格，或者大小写有别。
验证方法：在开发者工具中，找到那个 iframe，右键 -> "Copy" -> "Copy outerHTML"，然后粘贴到一个文本编辑器里仔细检查 title 的内容。
