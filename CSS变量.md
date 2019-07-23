# CSS文件里的定义以及引用
## 定义
--* (变量名貌似也可以用中文)
```
:root {
  --main-bg-color: pink;
}
```
## 引用
var(--*)
```
body {
  background-color: var(--main-bg-color);
}
```

## 通过js定义变量
### css/main.css
```
body {
    margin: 0;
    background: var(--bgColor);
}

.card{
    background: var(--bgWhite);
}
```
### index.html
```
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="css/main.css">
        <title>playground</title>
    </head>
    <body>

        <div class="card">
            It's a card.
        </div>
        
        <script>
            document.body.style.setProperty('--bgColor', 'black');
            document.body.style.setProperty('--bgWhite', 'white');
        </script>
    </body>
</html>
```
# 兼容性
不支持IE其他基本都支持
[https://developer.mozilla.org/en-US/docs/Web/CSS/var](https://developer.mozilla.org/en-US/docs/Web/CSS/var)
