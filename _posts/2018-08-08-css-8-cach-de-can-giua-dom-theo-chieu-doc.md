---
layout: post
title:  "CSS: 8 cách để căn giữa DOM theo chiều dọc"
author: po
# categories: [ tutorial ]
image: assets/img/posts/vertical-centering/vertical-centering.jpg
# featured: false
# hidden: true
tags: [css, vertical-align, vertical-centering, guide]
---


## Vertical-align:
Đã bao nhiêu lần bạn đã thử dùng `vertical-align: middle;` để thử căn giữa một DOM theo chiều dọc? Mình thì ít nhất là chục lần!
Thoạt nhìn thì có vẻ logic, nhưng loay hoay một lúc thì phát hiện nó chả hoạt động gì?! 

Chắc bạn (và mình) cũng đã từng nghĩ, tại sao căn giữa theo chiều ngang (khá) dễ  thì chiều dọc lại làm khó mình vậy nhỉ???
Thực ra mình cũng đ** hiểu lắm và có vẻ nhiều dev khác cũng cảm thấy vậy! Và nhiều cách "work around" đã được nghĩ ra để giải quyết (một cách không triệt để lắm) vấn đề này, ví dụ như: _table_, _float_, _inline-block_, _position_.

Hôm nay mình muốn cùng thử tất cả 8 cách (mà mình từng google và được biết) để giải quyết việc căn giữa theo chiều dọc một DOM.

------------------------

## 1. line-height
_Ghi nhớ là kỹ thuật này chỉ áp dụng cho **1 dòng text**_

Hãy thử với một vài dòng html và css như sau:

**html**

```html
<div id="element">Text</div>
<div id="element-2">
  <img src="https://cdn.phohuynh.com/github.png"></img>
</div>
```

**css**

```css
#element {
    background-color: #ddd;
    height: 150px;
    line-height: 150px;
}
#element-2 {
    background-color: #eee;
    height: 150px;
    line-height: 150px;
}
#element-2 img {
    height: 80px;
    vertical-align: middle;
}
```

**Kết quả:**
<p data-height="400" data-theme-id="0" data-slug-hash="RBxbNE" data-default-tab="css,result" data-user="shortgiraffe4" data-pen-title="line-height" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/RBxbNE/">line-height</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

**Chú ý**: 
_line-height_ phải lớn hơn _font-size_ (và _height_ của image).

Thường thì bạn không cần phải set _height_ của _child_ element vì _line-height_ sẽ đẩy chiều của của div lên nhưng nếu không hoạt động hoặc cảm thấy không tự tin thì set _height_ nhé!

Với image bạn phải thêm 1 dòng `vertical-align: middle` nữa thì cách này mới hoạt động.

------------------------

## 2. Table

**html**

```html
<div id="parent">
  <div id="child">Content</div>
  <div id="child-2">Content 2</div>
</div>
```

**css**

```css
#parent {
  display: table;
  background-color: #ddd;
  width: 100%;
}
#child {
  display: table-cell;
  vertical-align: middle;
}
#child-2 {
  height: 150px;
}
```

Ở đây để cho dễ hình dung mình dùng 1 div là _#parent_ và 2 div là _#child_ và _#child-2_.

Chỉ có _#child_ là được căn giữa thôi nhé!

**Kết quả:**
<p data-height="300" data-theme-id="0" data-slug-hash="OwZGOp" data-default-tab="css,result" data-user="shortgiraffe4" data-pen-title="Table" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/OwZGOp/">Table</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

------------------------

## 3. Positioning và stretch technique

Hãy nhìn và đoạn code dưới đây:

**html**

```html
<div id="parent">
  <div id="child"></div>
</div>
```

**css**

```css
#parent {
  position: relative;
  height: 150px;
  background-color: #eee;
}
#child {
  position: absolute;
  top: 0;
  right: 0;  
  bottom: 0;
  left: 0;
  width: 50%;
  height: 30%;
  margin: auto;
  background-color: cyan;
}
```

Ở đây đầu tiên mình set `position` của _#parent_ và _#child_ lần lượt là `relative` và `absolute`. 

Sau đó mình dùng kỹ thuật stretch, set lần lượt `top`, `right`, `bottom`, `left` là `0` (thứ tự không quan trọng) thì _#child_ sẽ được căn chính giữa _#parent_.

**Kết quả:**
<p data-height="265" data-theme-id="0" data-slug-hash="gjKwGM" data-default-tab="css,result" data-user="shortgiraffe4" data-pen-title="Positioning and stretch" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/gjKwGM/">Positioning and stretch</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

------------------------

## 4. Padding top và padding bottom bằng nhau

Cách này hoạt động khá đơn giản nhưng yêu cầu khá...khắt khe một tí.

Thử với code như sau: 

**html**
```html
<div id="parent">
  <div id="child"></div>
</div>
```

**css**
#parent {
  background-color: #eee;
  padding: 5% 0;
}
#child {
  height: 50px;
  background-color: cyan;
}

<p data-height="265" data-theme-id="0" data-slug-hash="djgOLG" data-default-tab="css,result" data-user="shortgiraffe4" data-pen-title="Equal top and bottom padding" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/djgOLG/">Equal top and bottom padding</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

Bạn thấy đó, dù bạn set padding của _#parent# như thế nào (chỉ cần top và bottom bằng nhau) thì chắc chắn _#child_ sẽ được căn giữa. 

Vì chiều cao của của _#parent_ sẽ giãn ra tùy theo _#child_ và luôn bằng `padding-top + padding-bottom + height (của _#child_)`. Và _padding-top_ === _padding-bottom_ nên nó sẽ ở giữa thôi!

Vậy nếu như chiều cao của _#parent_ không chỉ phụ thuộc vào một mình _#child_ thì sao? Vậy thì sẽ xảy ra 2 trường hợp:
1. Biết trước chiều cao của _#parent_, vậy thì hãy tìm một số padding thích hợp để `height (của _#parent_) === padding-top + padding-bottom + height (của _#child_))`
2. Chiều cao của _#parent_ không thể xác định được vì phụ thuộc vào chiều cao của một element khác, ví dụ _#child-2_. Vậy thì xác định là không dùng được cách này!

------------------------

## 5. Dùng một div floater rỗng

Cách này, hmm, khá là "tricky", hãy thử với code sau:

**html**
```html
<div id="parent">
  <div id="float"></div>
  <div id="child">Child</div>
</div>
```

**css**
```css
#parent {
  height: 250px;
  background-color: #eee;
}
#float {
  float: left;
  height: 50%;
  width: 100%;
  background-color: khaki;
  margin-bottom: -50px;
  position: relative;
  z-index: 1;
}
#child {
  clear: both;
  height: 100px;
  background-color: cyan;
  position: relative;
  z-index: 2;
}
```

**Kết quả**
<p data-height="465" data-theme-id="0" data-slug-hash="gjBggX" data-default-tab="css,result" data-user="shortgiraffe4" data-pen-title="Floater div" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/gjBggX/">Floater div</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

2 điều kiện để cách này hoạt động đó là phải có một div rỗng ở trước _#child_ và phải xác định được chiều cao của _#child_, sau đó set _margin-bottom_ của _#float_ bằng âm 1 nửa chiều cao của _#child_ (-1/2 height).

_Về phần position và z-index mình chỉ làm cho các bạn nhìn rõ thôi chứ trong cách này chỉ cần #float rỗng là được._

------------------------

## 6. Inline-block và pseudo element

Cách này so với cách 5 còn có khi...kỳ quặc hơn nữa:

**html**

```python
<div id="parent">
  <div id="child">
    Lorem ipsum dolor sit, amet consectetur adipisicing elit. Excepturi labore nulla fugit sapiente obcaecati aliquid tenetur perferendis, quo unde quaerat dolores maiores nostrum ab? Et atque ex quibusdam magni corrupti.
  </div>
</div>
```

**css**

```css
#parent {
  position: relative;
  background-color: #eee;
  height: 300px;
}
#parent::before {
  content: "";
  display: inline-block;
  height: 100%;
  width: 0;
  vertical-align: middle;
}
#child {
  display: inline-block;
  vertical-align: middle;
  width: 99%;
}
```

**Kết quả**

<p data-height="300" data-theme-id="0" data-slug-hash="jpeBBP" data-default-tab="html,result" data-user="shortgiraffe4" data-pen-title="Inline-block and pseudo element" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/jpeBBP/">Inline-block and pseudo element</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

Cách này mình cần tạo ra một _pseudo_ element (`::before`) với chiều cao bằng 100% của _#parent_, vị trí của nó sẽ là ngay đằng trước _#child_ và giúp nó align.

_Note: width của #child chỉ có thể nhỏ hơn hoặc bằng 99% width của #parent, nếu không nó sẽ bị đẩy ra khỏi #parent._

------------------------

## 7. Flex-box

Những cách trên hầu hết là _hack_ để có thể căn giữa, nhưng _flex-box_ được sinh ra, mục đích của nó _có thể xem_ là muốn giải quyết vụ căn giữa này! 

Nhưng mà _flex-box_ có quá nhiều tính năng và hầu như là quá phức tạp so với việc chỉ dùng để căn giữa thôi. Cho mình tốt nhất bạn hãy bỏ ra một ít thời gian để tìm hiểu về những thứ hay ho của _flex-box_, đảm bảo giá trị luôn!

**html**

```html
<div id="parent">
  <div id="child"></div>
</div>
```
**css**

```css
#parent {
  height: 200px;
  background-color: #eee;
  display: flex;
  justify-content: center; /** Căn giữa chiều ngang */
  align-items: center; /* Căn giữa chiều dọc */
}
#child {
  width: 100px;
  height: 50px;
  background-color: cyan;
}
```

**Kết quả**

<p data-height="300" data-theme-id="0" data-slug-hash="gjBgdK" data-default-tab="css,result" data-user="shortgiraffe4" data-pen-title="Flexbox" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/gjBgdK/">Flexbox</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

Cách này hoạt động với _mọi kích thước_ của _#parent_ và _#child_, nhưng nó không hỗ trợ mọi trình duyệt, bạn có thể xem những trình duyệt hỗ trợ tại đây: <https://developer.mozilla.org/en-US/docs/Web/CSS/flex>.

Còn đây là full guide của flexbox: <https://css-tricks.com/snippets/css/a-guide-to-flexbox/>.

------------------------

## 8. Grid

Grid (Grid layout) là một chức năng sinh ra để giải quyết về...layout. Đây là cách cuối cùng của bài và cũng là cách "chính thức" nhất dùng để căn giữa theo chiều dọc.

**html**

```html
<div id="parent">
  <div id="child"></div>
</div>
```

**css**

```css
#parent {
  height: 200px;
  background-color: #eee;
  display: grid;
  justify-items: center; /* Căn giữa chiều ngang */
  align-content: center;/* Căn giữa chiều dọc */
}
#child {
  width: 100px;
  height: 50px;
  background-color: cyan;
}
```

**Kết quả**

<p data-height="265" data-theme-id="0" data-slug-hash="qyJrEP" data-default-tab="css,result" data-user="shortgiraffe4" data-pen-title="Grid" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/qyJrEP/">Grid</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>

Wow, trông có vẻ giống _flex-box_. Nhưng tính ra thì _grid_ đơn giản hơn so với _flex-box_, không tin thì google nhé :D

Hỗ trợ trình duyệt: <https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout>.


<script src="https://static.codepen.io/assets/embed/ei.js"></script>