---
layout: post
title:  "Tạo tooltip chỉ với HTML và CSS"
author: po
# categories: [ tutorial ]
image: assets/img/posts/tooltip/tooltip.png
# featured: false
# hidden: true
tags: [html, css, tooltip]
---

## Tooltip
Mình đã từng tìm rất nhiều thư viện cũng như plugin (cho jQuery) để chỉ tạo tooltip đơn giản. Và mình (và nhiều bạn dev khác) đã thành công và tìm ra những công cụ hữu ích như:
Tooltip mặc định của `Bootstrap`, của `jQuery UI`, hay thư viện chuyên nghiệp như `popper.js`. Nhưng thực tế, không cần động tới Javascript, chúng ta vẫn hoàn toàn có thể tạo được 1 tooltip rất dễ dàng!

## Pseudo Element

Để làm được điều này chúng ta sẽ nhờ đến sự trợ giúp của `pseudo element` đó là `::before` hoặc `::after`.

Còn về `pseudo element` là gì bạn có thể đọc tại đây: [Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)

## Code

Mục tiêu của bài viết là tạo ra một tooltip với:
- Flexible content: chỉ có 1 đoạn code css áp dụng cho tất các element mà không cần phải viết riêng cho từng element riêng biệt (nội dung tooltip khác nhau).
- Hiển thị khi hover và có vị trí phía trên, bên phải của element.
- Có thể có hiệu ứng hiển thị khi hover.

Bây giờ mình chỉ cần một anchor đơn giản như sau và bắt đầu chỉnh style cho nó:

```html
<a href="https://phohuynh.com" data-tooltip="A cool tooltip!">A cool link &#x1F984;!</a>
```

Với mục tiêu là _flexible content_ chúng ta sẽ dùng attribute `data-tooltip` (có thể đặt attribute tên khác, không nhất thiết phải là `data-tooltip`).

Tiếp theo chúng ta sẽ tạo một tooltip bằng pseudo element với css:

```css
a[data-tooltip]::after {
    content: attr(data-tooltip); /* Nội dung tooltip lấy từ anchor */
}
```

Đến đây chúng ta sẽ có kết quả như thế này:

![Step 1](/assets/img/posts/tooltip/step1.png "Step 1")

Hiện tại tooltip sẽ hiện ra phía bên phải của anchor mà không có style gì, nhưng chúng ta đã có nội dung tooltip flexible rồi!

Giờ mình sẽ chỉnh style cho nó:

```css
a[data-tooltip] {
    position: relative;
    display: inline-block;
}

a[data-tooltip]::after {
    content: attr(data-tooltip); /* Nội dung tooltip lấy từ anchor */
    display: block;
    padding: 1em 3em;
    border-radius: 4px;
    background-color: rgba(0, 0, 0, 0.7);
    color: #fff;
    white-space: nowrap;
    position: absolute;
    bottom: 110%;
    left: 0;
    transform: translateY(5px);
    opacity: 0;
    transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
}

a[data-tooltip]:hover::after {
    opacity: 1;
    transform: translateY(0);
}
```

Và đây là kết quả sau khi chỉnh style:

![Step 2](/assets/img/posts/tooltip/tooltip.gif "Step 2")

Codepen version:
<p data-height="265" data-theme-id="0" data-slug-hash="JaaZBJ" data-default-tab="result" data-user="shortgiraffe4" data-pen-title="Tooltip with only html and css" class="codepen">See the Pen <a href="https://codepen.io/shortgiraffe4/pen/JaaZBJ/">Tooltip with only html and css</a> by Pho Huynh (<a href="https://codepen.io/shortgiraffe4">@shortgiraffe4</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script src="https://static.codepen.io/assets/embed/ei.js"></script>

Vậy là chúng ta đã có một tooltip đơn giản nhưng không cần Javascript và có lẽ sẽ giảm thiểu được các vấn đề về performance khi có một lượng lớn tooltip trên document nếu phải sử dụng Javascript!

Happy coding!