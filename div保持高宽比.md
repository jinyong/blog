# 利用定位
```
.wrapper{
  position : relative;
  background: #ccc;
  width: 10%;
  padding-bottom : 20%;
}
.inner{
  position : absolute;
  top : 0; left : 0; right : 0; bottom : 0;
}
</style>

<div class="wrapper">
    <div class="inner">
        这是内容
    </div>
</div>
```
两层结构。而且inner div的高度，即使设置为100%，也不能填充父div，有可能是定位的关系。
# 利用伪元素
```
<style>
.wrapper {
  background: #ccc;
  width: 10%;
}
.wrapper::before {
  content: '';
  padding-top: 200%;
  float: left;
}
.wrapper::after {
  content: '';
  display: block;
  clear: both;
}
</style>
<div class="wrapper">
    这是内容
</div>
```
一层结构。放入子div，并设置高度为100%，还是不能填充父div。
