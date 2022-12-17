---
sidebar_position: 1
---
# HTML

## 语意标签化

语义化就是是让机器可以读懂内容，web页面的解析是由搜索引擎来进行搜索，机器来解析。

header 头 section 部分 footer 脚

article 内容

nav 导航

aside 一边

## 页面布局

CSS布局：table布局、float布局、flex布局、inline- block布局

## 元素分类

行内元素：a、b、span、img、input、strong、select、label、em、button、textarea 

块级元素：div、ul、li、dl、dt、dd、p、h1-h6、blockquote 

空元素:br、meta、hr、link、input、img


```js
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
}
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
    display: block;
}
body {
    line-height: 1;
}
ol, ul {
    list-style: none;
}
blockquote, q {
    quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
    content: '';
    content: none;
}
table {
    border-collapse: collapse;
    border-spacing: 0;
}
```

