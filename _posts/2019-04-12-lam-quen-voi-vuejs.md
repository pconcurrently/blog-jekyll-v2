---
layout: post
title:  "Làm quen với Vue.js trong 5 phút"
author: po
# categories: [ tutorial ]
image: assets/img/posts/vue/vue.png
# featured: false
# hidden: true
tags: [vue, vuejs]
---

## Giới thiệu sơ về Vue.js
Ok, có lẽ bạn đã nghe qua về `Vue.js` ít nhất là một (vài) lần và có mong muốn biết thêm về *framework* này.
Điều đầu tiên bạn cần biết, `Vue.js` là một *framework* trong khi `React.js` là một *library* chuyển xử lý view layer (UI).

Nếu như bạn từng làm việc với React thì bạn sẽ để ý sự khác biệt rõ ràng giữa `Vue.js` và `React.js`: React yêu cầu dev phải tích hợp (integrate) các thư việc thứ 3 (3rd party package) thì mới có thể hoạt động trơn tru và hoàn chỉnh, trong khi `Vue.js` cung cấp sẵn những thư viện như vậy rồi.

Cả 2 React và Vue đều tận dụng `Virtual DOM` để tăng khả năng xử lý DOM.

Bạn có thể tìm hiểu thêm rất nhiều bài viết trên mạng về việc so sánh `Vue.js` và các framework/library khác. Hiện tại mình sẽ giới thiệu cách tạo một app ToDo đơn giản để những bạn muốn tìm hiểu `Vue.js` có cái nhìn tổng quan về framework khá được cộng đồng thích thú này.

## TODO App
Cũng nhưng React, `Vue.js` cung cấp một tool CLI giúp cho việc setup project (`create-react-app`): `vue-cli`.

Bắt đầu bằng việc cài `vue-cli`:
```
npm install -g @vue/cli
```
Khởi tạo project:
```
vue create todo-vue
cd todo-vue
code .
```
(Mở project bằng VSCode, nếu bạn chưa xài thì hãy xài đi!)

Bây giờ tạo một file `ToDoItem.vue` ở `src/components/ToDo/ToDoItem.vue`:

```vue
<template>
  <div class='todo-item'>
    <p class='todo-item__text'>{% raw %}{{todo.text}}{% endraw %}</p>
    <div class='todo-item__delete' @click='deleteItem(todo)'>-</div>
  </div>
</template>

<script>
  export default {
    name: 'to-do-item',
    props: ['todo'],
    methods: {
      deleteItem(todo) {
        this.$emit('delete', todo);
      },
    },
  };
</script>

// npm install -D sass-loader node-sass
<style lang="scss">
  .todo-item {
    display: flex;
    align-items: center;
    justify-content: center;

    * {
      padding: 6px;
    }

    &__delete {
      cursor: pointer;
    }
  }
</style>
```
Giải thích:
`ToDoItem.vue` chia làm 3 phần rõ rệt `template`, `script` và `style`.

`template`: Cú pháp này cơ bản là thêm markup vào trong HTML, khá giống với Angular 1.

`script`: 
 - `name`: tên của component, chủ yếu dùng để component tự gọi bảng thân (recursive), tạm thời thì biến này không có tác dụng gì trong ví dụ này, bạn có thể đổi thành bất cứ gì mà nó vẫn chạy như bình thường
 - props: tương tự như props của `React.js`, đây là những data được pass từ component parent, vậy chúng ta sẽ kỳ vọng component parent của `ToDoItem` sẽ truyền cho nó một object `todo`.
 - `methods`: nơi định nghĩa (define) các function của component.

`style`: ở đây bạn có thể thấy là mình sử dụng cú pháp của `SASS` để viết chứ không phải `CSS` thuần. Bạn hoàn toàn có thể viết bằng `CSS`, chỉ cần bỏ `lang="scss"` đi thôi. Nhưng nếu muốn viết bằng `SASS` thì bạn phải chạy command sau:

```
npm install -D sass-loader node-sass
```
Còn lại thì `webpack` sẽ tự động xử lý, khá tiện lợi!

Một feature thú vị nữa là nếu bạn thêm như thế này: `<style lang="scss" scoped>` thì style định nghĩa ở đây sẽ chỉ có tác dụng trong componen này thôi (Khá giống với một tính năng của Angular 2 bảng beta mình từng làm).



Tiếp theo chúng ta sẽ tạo component parent của `ToDoItem.vue`, là `ToDo.vue` ở `src/components/ToDo/ToDo.Vue`:

```vue
<template>
  <div class='todo-container'>
    <div class='todo'>
      <h1 class='todo__header'>Vue ToDo</h1>
      <div class='todo__container'>
        <div class='todo__content'>
          <ToDoItem v-for='todo in list' :todo='todo' @delete='onDeleteItem' :key='todo.id' />
        </div>
        <div class='todo__form'>
          <input type='text' v-model='todo' v-on:keyup.enter='createNewToDoItem'>
          <input type="submit" class='todo__add' value='+' @click="createNewToDoItem"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ToDoItem from './ToDoItem';

export default {
  name: 'to-do',
  components: {
    ToDoItem,
  },
  data() {
    return {
      list: [
        {
          id: 1,
          text: 'create vue todo',
        },
        {
          id: 2,
          text: 'create react todo',
        },
      ],
      todo: '',
    };
  },
  methods: {
    createNewToDoItem() {
      if (!this.todo) {
        // eslint-disable-next-line
        alert('Please enter a todo!');
        return;
      }
      const newId = Math.max.apply(null, this.list.map(t => t.id)) + 1;
      this.list.push({ id: newId, text: this.todo });
      this.todo = '';
    },
    onDeleteItem(todo) {
      this.list = this.list.filter(item => item !== todo);
    },
  },
};
</script>

// 'scoped' -> only applied to this file
<style lang="scss" scoped>
  @import '../../styles/_colors';

  .todo {
    display: block;

    &__add {
      color: $main;
      font-size: 18px;
    }
  }

</style>
```

Ở đây mình có tạo thêm một file scss là `src/styles/_colors.scss` để thử xem import có dễ không thôi 😄

Không có gì nhiều để giải thích ngoại trừ 1 điểm khá thú vị. Như đã đề cập phía trên, `Vue.js` và `React.js` đều sử dụng `Virtual DOM`, nhưng `React.js` bắt buộc việc thay đổi (mutate) `state` thông qua function `setState` mà không được thay đổi trực tiếp, việc này giúp React trong việc chạy function `render` để tạo Virtual DOM. Nhưng mà `Vue.js` lại cho phép thay đổi `state` trực tiếp:

```javascript
const newId = Math.max.apply(null, this.list.map(t => t.id)) + 1;
this.list.push({ id: newId, text: this.todo });
this.todo = '';
```

`Vue.js` tự động xử lý chỗ này, có vẻ như `Vue.js` thêm một chút *magic* cho dev 😌 Nhưng tại sao `React.js` không làm như vậy? Dường như `React.js` cũng có lý do để làm vậy! Google thêm nhé!

Bước cuối cùng chỉ là thêm component `ToDo.vue` vào `router` để nó hiển thị khi chạy app thôi.

```javascript
import Vue from 'vue';
import Router from 'vue-router';
import ToDo from '@/components/ToDo/ToDo';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/',
      name: 'To Do',
      component: ToDo,
    },
  ],
});
```

Chạy app bằng command `npm start` và xem kết quả!

*Bài tiếp theo mình sẽ so sánh `React.js` và `Vue.js` khi tạo cùng 1 app ToDo.*

Happy Coding!
