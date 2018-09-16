---
layout: post
title:  "CSS: Tạo chữ trong suốt đơn giản"
author: po
# categories: [ tutorial ]
image: assets/img/posts/css-transparent-text/transparent.jpg
# featured: false
# hidden: true
tags: [css, transparent-text, chu-trong-suot, huong-dan]
---

## Chữ trong suốt?

Thực ra không hề có chữ trong suốt trong CSS, mà ở đây chúng ta sẽ lợi dụng `blending mode` của CSS để tạo ra hiệu ứng này.
Nếu như đã từng "chơi" với Photoshop thì bạn cũng có thể biết đến cụm từ `blending mode` này vì thực chất đó là một.

Hôm nay mình sẽ hướng dẫn các bạn tạo ra hiệu ứng này với vài dòng code đơn giản.

## Thực hiện

Chúng ta sẽ có 1 đoạn html và css đơn giản như sau:

**html**

```html
<div id="container">
    <h1>Transparent text</h1>
</div>
```

**css**

```css
#container {
    background-image: url("https://cdn.phohuynh.com/cat.jpg");
    background-size: cover;
    background-repeat: none;
    width: 100vw;
    height: 100%;
    padding: 40px 0;
}
h1 {
    display: block;
    padding: 40px;
    font-size: 7vw;
    font-weight: 900;
    text-align: center;
    text-transform: uppercase;
    background-color: black;
    color: white;
    mix-blend-mode: multiply; /* Magic happens here */
}
```

**Kết quả**
<p data-height="383" data-theme-id="0" data-slug-hash="jvvVrR" data-default-tab="result" data-user="shortgiraffe4" data-pen-title="Blending mode css" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/jvvVrR/">Blending mode css</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script src="https://static.codepen.io/assets/embed/ei.js"></script>

## Giải thích:
Để cách này hoạt động được thì ngoài `mix-blend-mode: multiply;` chúng ta còn phải kể tới `background-color: black;` và `color: white;`.

Bạn cũng có thể tưởng tượng ra một chút, `blending mode` là `multiply` thì nó sẽ `nhân` chỉ số rgb của 2 lớp layers với nhau, một là hình và một là chữ.

Ví  dụ chúng ta có 2 layers với màu là: 

Layer 1: `rgb(a%, b%, c%)`

Layer 2: `rgb(x%, y%, z%)`

Thì kết quả sẽ là `rgb(a% * x%, b% * y%, c% * z*)`

Vậy điều gì sẽ xảy ra với màu đen và màu trắng?

Màu trắng: `rgb(100%, 100%, 100%)`

Màu đen: `rgb(0%, 0%, 0%)`

Nhìn qua thì có thể thấy được, màu nào `multiply` với màu trắng thì sẽ thành chính nó, và màu nào `multiply` cho màu đen thì sẽ vẫn là màu đen!

Cho nên phần chữ màu trắng (`color: white`) của chúng ta sẽ có màu của background ngay dưới nó và phần nền màu đen (`background-color: black`) sẽ giữ nguyên như thế.

Happy coding!