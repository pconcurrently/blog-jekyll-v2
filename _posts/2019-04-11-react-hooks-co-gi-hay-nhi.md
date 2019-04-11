---
layout: post
title:  "React Hooks có gì hay nhỉ?"
author: po
# categories: [ tutorial ]
image: assets/img/posts/hooks/hooks.png
# featured: false
# hidden: true
tags: [react, react-hooks]
---

## React Hooks là gì?
React Hooks là một feature mới được chính thức release trong bản React 16.8.
Như chúng ta đã biết thì trong React có 2 loại component trong React là Stateful và Stateless component. Với React Hooks thì chúng ta có thể sử dụng state và nhiều feature khác trong một stateless component (What?!!).

## Tại sao chúng ta cần React Hooks?
Khoảng thời gian 1 năm trước, React Team bắt đầu nhận các proposal thông qua một RFC process (Request for comments). Và một trong các proposal đó đã được implement, test và trở thành một feature chính thức ở phiên bản 16.8 này, React Hooks.
Sau một thời gian làm việc với React thì có lẽ chúng ta sẽ bắt gặp một trong số các vấn đề sau
 - "Wrapper hell" các component được lồng (nested) vào nhau nhiều tạo ra một DOM tree phức tạp.
 - Component quá lớn.
 - Sự rối rắm của class

React Hooks được sinh ra với mong muốn giải quyết những vấn đề nổi cuộm này. Như thế nào? React Hooks cho phép share stateful logic gữa các component => reuse và dễ test.

## Cái nhìn đầu tiên về React Hooks

### useState
Đây là cách bình thường chúng ta sử dụng state trong một component:
```jsx
import React from 'react';

class Example extends React.Component {
  constructor(props){
    super(props);

    this.state = {
      count: 0,
    };
  }

  setCount = () => {
    const { count } = this.state;
    this.setState({
      count: count + 1,
    });
  }

  render() {
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={this.setCount}>
          Click me
        </button>
      </div>
    );
  }
}
```

Và đây là cách chúng ta implement với React Hooks
```jsx
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
Ở đây `useState` là một `hook`. Qua ví dụ thì các bạn cũng có thể thấy, thực chất `hook` cũng chỉ là một function. Và function này cho phép chúng ta mang state vào một stateless component.
`useState` nhận một parameter duy nhất là giá trị ban đầu của state.
```jsx
const [count, setCount] = useState(0);
```
Ở đây chúng ta sử dụng `array destructing` để gán tên cho state thông qua việc gọi `useState`.

### useEffect
Khi chúng ta cần gọi api để lấy data, khai báo eventListener, thay đổi DOM,...các `side effect` này được xử lý bằng cách sử dụng `component life-cycle api` của React. Ví dụ như:

```jsx
import React from 'react';

class Example extends React.Component {
  constructor(props){
    super(props);

    this.state = {
      count: 0,
    };
  }

  componentDidMount() {
    document.title = `You clicked ${count} times`;
  }

  componentDidUpdate() {
    document.title = `You clicked ${count} times`;
  }

  setTitle = (e) => {
    const { count } = this.state;
    this.setState({
      count: count + 1,
    });
  }

  render() {
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={this.setCount}>
          Click me
        </button>
      </div>
    );
  }
}
```

Và đây cách làm tương đương khi sử dụng React Hooks:
```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [title]);

  render() {
    return (
      <input name="title" value={title} onChange={() => setCount(count + 1)} />
    );
  }
}
```
Khá đơn giản phải không?!
Điều bạn cần chú ý ở đây là 3 dòng code này:
```jsx
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [title]);
```
`useEffect` nhận 2 parameter, đầu tiên là một function nơi chúng ta xử lý các `side effect`, thứ hai là một array `[title]`.
Chúng ta có thể hiểu array này là nơi chứa những variable(biến) (không nhất thiết là state), mà khi những variable này thay đổi thì hook `useEffect` này sẽ được kích hoạt (chạy, execute).
Khi chúng ta không bỏ array này vào hook `useEffect` thì nó sẽ chạy cùng với mọi lần component chạy function `render`.

```jsx
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
```

Còn nếu như chúng ta chỉ muốn nó chạy 1 lần sau lần render đầu tiên thì sao? Đây:
```jsx
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, []);
```
Chỉ cần array là một array rỗng là được!

Một ví dụ khác khi sử dụng `useEffect` là khi chúng ta cần phải *clean up* những gì mà chúng ta đã subscribe.

Sử dụng class:
```jsx
import React from 'react';

class Example extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      scrollTop: 0,
    };
  }
  componentDidMount() {
    window.addEventListener('scroll', this.handleScroll);
  }

  componentWillUnmount() {
    window.removeEventListener('scroll', this.handleScroll);
  }

  handleScroll = (e) => {
    this.setState({
      scrollTop: e.srcElement.scrollingElement.scrollTop,
    });
  }

  render() {
    const dStyle = { height: "4000px" };
    const hStyle = { position: 'fixed' };
    return (
      <div style={dStyle}>
        <h1 style={hStyle}>{`scrollTop is: ${scrollTop}`}</h1>
      </div>
    );
  }
}
```
Sử dụng hooks:

```jsx
import React, { useEffect, useState } from 'react';

function Example() {
  const [scrollTop, setScrollTop] = useState(0);

  useEffect(() => {
    window.addEventListener('scroll', handleScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
    }
  });

  function handleScroll(e) {
    setScrollTop(e.srcElement.scrollingElement.scrollTop);
  }

  const dStyle = { height: "4000px" };
  const hStyle = { position: 'fixed' };

  return (
    <div style={dStyle}>
      <h1 style={hStyle}>{`scrollTop is: ${scrollTop}`}</h1>
    </div>
  );
}
```

=> Thay vì như trước đây, code bị phân tán ra và nằm ở những life-cycle khác nhau thì chúng ta có thể bỏ tất cả logic liên quan đến một chức năng vào một chỗ để dễ quản lý hơn! Cool!

Chúng ta đã làm quen với 2 hook cơ bản nhất trong số các hook. Nhưng như đã nói, `React Hooks` cho phép share stateful logic giữa các component, với những code example như trên thì làm sao chúng ta có thể làm được? Đừng lo, `React Hooks got you 😉`.

### Custom Hooks
```jsx
import React, { useEffect, useState } from "react";

function useScroll() {
  const [scrollTop, setScrollTop] = useState(0);

  function handleScroll(e) {
    setScrollTop(e.srcElement.scrollingElement.scrollTop);
  }

  useEffect(() => {
    window.addEventListener("scroll", handleScroll);

    return () => {
      window.removeEventListener("scroll", handleScroll);
    };
  });

  return scrollTop;
}

function Example() {
  const scrollTop = useScroll();
  const dStyle = { height: "4000px" };
  const hStyle = { position: 'fixed' };

  return (
    <div style={dStyle}>
      <h1 style={hStyle}>{`scrollTop is: ${scrollTop}`}</h1>
    </div>
  );
}
```

`useScroll` là `custom hook` mà chúng ta mong muốn. Nó có thể đặt ở một file riêng biệt và được sử dụng ở những component khác nhau nếu như chúng ta có nhu cầu!

Còn những `hook` nào nữa? Well, còn một cơ số các `hook` nữa mà chúng ta có thể sử dụng, ví dụ như `useContext`, `useReducer`, `useRef`,...
Các bạn có thể tham khảo thêm tại: [https://reactjs.org/docs/hooks-reference.html](https://reactjs.org/docs/hooks-reference.html)

Cuối cùng, có một thắc mắc mà nhiều người cũng tự hỏi khi tìm hiểu về `React Hooks`:
Nếu như chúng ta có một cơ số các state thì chúng ta phải khai báo như thế này à???
```jsx
const [countA, setCountA] = useState(0);
const [countB, setCountB] = useState(0);
const [countC, setCountC] = useState(0);
const [countD, setCountD] = useState(0);
const [countE, setCountE] = useState(0);
const [countF, setCountF] = useState(0);
const [countX, setCountX] = useState(0);
// ...
```
Well, đó cũng là *một cách*. Gợi ý: Bạn có thể sử dụng `useReducer` để khắc phục tình trạng này. Các bạn tự tìm hiểu thêm nhé!

Nâng cao hơn nữa, chúng ta còn có thể quản lý state mà không cần redux:
[https://levelup.gitconnected.com/implementing-redux-style-state-management-with-reacts-usecontext-usereducer-hooks-c1c5596d9619](https://levelup.gitconnected.com/implementing-redux-style-state-management-with-reacts-usecontext-usereducer-hooks-c1c5596d9619)

Happy Coding!
