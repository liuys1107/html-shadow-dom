###Web Component
>Shadow DOM是HTML的一个规范 ，它允许浏览器开发者封装自己的HTML标签、CSS样式和特定的javascript代码，同时也可以让开发人员创建类似<video>这样的自定义一级标签，创建这些新标签内容和相关的的API被称为Web Component。

####主界面
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="import" href="my-lys.html" >
</head>
<body>
    <my-lys message="hello, world"></my-lys>
</body>
</html>
```
####Shadow DOM组件
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <template id="lys">
        <style>
            h1{
                color: cyan;
                text-align: center;
            }
        </style>
        <h1 id="info"></h1>
    </template>
    <script>
        // 第一步
        var indexDoc = document; // 代表的是index.html的文档的对象;

        // currentScript获取正在运行的script标签对象
        // ownerDocument 我们的文档
        var currentDoc = indexDoc.currentScript.ownerDocument;
        var tmp = currentDoc.getElementById('lys');
        // 第二步 注册元素
        // 继承html家族

        var lysProto = Object.create(HTMLElement.prototype);
        // 第三步
        // lysProto注册，他有个回调相当于 ajax onreadystatechange
        // 如果有元素注册成功就会调用执行方法
        lysProto.createdCallback = function () {
            // 创建一个 shadow Dom
            var root = this.createShadowRoot();
            root.appendChild(indexDoc.importNode(tmp.content, true));
            root.getElementById('info').innerHTML = this.getAttribute("message");
            // this.innerHTML = tmp.innerHTML;
        };

        indexDoc.registerElement("my-lys", {
            prototype: lysProto,
        })

    </script>
</body>
</html>
```
