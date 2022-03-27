### 2022/3/27

### 【css】三栏布局有哪些方式

定义：左右固定宽度，中间自适应的布局

- float+margin

  容器内的元素需要按照`左、右、中`的方式依次排列

  因为 float 只会相对于后面的元素浮动，并不会影响前面的元素

  ```html
  <title>float+margin实现三栏布局</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }

      .container {
        height: 500px;
        background-color: antiquewhite;
      }

      .left {
        float: left;
        width: 200px;
        background-color: aqua;
        height: 100%;
      }

      .main {
        background-color: red;
        margin-left: 200px;
        margin-right: 200px;
      }

      .right {
        float: right;
        width: 200px;
        background-color: rgb(64, 17, 78);
        height: 100%;
      }
    </style>
  </head>

  <body>
    <div class="container">
      <div class="left"></div>
      <div class="right"></div>
      <div class="main">
        main
      </div>
    </div>
  </body>
  ```

- flex

  ```html
    <title>三栏布局之flex</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }

      .container {
        display: flex;
        height: 500px;
        background-color: antiquewhite;
      }

      .left {
        width: 200px;
        background-color: aqua;
        height: 100%;
      }

      .main {
        background-color: rgb(241, 239, 92);
        flex: 1;
      }

      .right {
        width: 200px;
        background-color: rgb(64, 17, 78);
        height: 100%;
      }
    </style>
  </head>

  <body>
    <div class="container">
      <div class="left"></div>
      <div class="main"></div>
      <div class="right"></div>
    </div>
  </body>
  ```

- float+BFC

  ```html
   <title>三栏布局之float+BFC</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }

      .container {
        height: 500px;
        background-color: antiquewhite;
      }

      .left {
        float: left;
        width: 200px;
        background-color: aqua;
        height: 100%;
      }

      .main {
        background-color: red;
        overflow: hidden;
        /* 给中间元素形成BFC */
      }

      .right {
        float: right;
        width: 200px;
        background-color: rgb(64, 17, 78);
        height: 100%;
      }
    </style>
  </head>

  <body>
    <div class="container">
      <div class="left"></div>
      <div class="right"></div>
      <div class="main">
        main
      </div>
    </div>
  </body>
  ```

- table

### BFC 是什么，有什么作用

一、什么是 BFC
BFC(block formatting context）：简单来说，BFC 就是一种属性，这种属性会影响着`元素的定位以及与其兄弟元素之间的相互作用`
中文常译为`块级格式化上下文`。是 W3C CSS 2.1 规范中的一个概念，它决定了`元素如何对其内容进行定位，以及与其他元素的关系和相互作用`。 在进行盒子元素布局的时候，BFC 提供了一个环境，在这个环境中按照一定规则进行布局不会影响到其它环境中的布局。比如浮动元素会形成 BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。 也就是说，如果一个元素符合了成为 BFC 的条件，`该元素内部元素的布局和定位就和外部元素互不影响，是一个隔离了的独立容器。`（在 CSS3 中，BFC 叫做 Flow Root）

二、 形成 BFC 的条件
1、浮动元素，float 除 none 以外的值；
2、绝对定位元素，position（absolute，fixed）；
3、display 为以下其中之一的值 inline-block、table-cell、flex、table-caption 或者 inline-flex
4、overflow 除了 visible 以外的值（hidden，auto，scroll）

三、BFC 常见作用
1、清除浮动元素
问题案例：高度塌陷问题：在通常情况下父元素的高度会被子元素撑开，而在这里因为其子元素为浮动元素所以父元素发生了高度坍塌，上下边界重合。这时就可以用 bfc 来清除浮动了。

2、不被浮动元素覆盖

```html
<style>
  .main {
    background-color: red;
    overflow: hidden;
    color: white;
  }

  #p1 {
    width: 300px;
    height: 200px;
    background-color: rgb(154, 154, 187);
    float: left;
  }

  #p2 {
    width: 200px;
    height: 100px;
    background-color: rgb(90, 0, 71);
  }
</style>

<div class="main">
  <div id="p1">p1我发生了浮动</div>
  <div id="p2">
    p2我没浮动，会被覆盖，必须给我添加 overflow: hidden，触发bfc来解决遮挡问题
  </div>
</div>
```

![](F:\github\front-end-knowledge\2022-03\2022-03-27\img\1.png)

问题案例： div 浮动兄弟遮盖问题：由于 p1 元素发生了浮动，所以和 p2 未发生浮动的块级元素不在同一层内，所以会发生 p1 将 p2 遮挡的问题。可以给 p2 加 overflow: hidden，触发 bfc 来解决遮挡问题。

![](F:\github\front-end-knowledge\2022-03\2022-03-27\img\2.png)

3、 BFC 会阻止外边距折叠
问题案例：margin 塌陷问题：在标准文档流中，块级标签之间竖直方向的 margin 会以大的为准，这就是 margin 的塌陷现象。可以用 overflow：hidden 产生 bfc 来解决。
