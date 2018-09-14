---
layout: post
title:  "Webpack 4 từ cơ bản cho đến xây dựng project React cùng ES6 để build production"
author: po
# categories: [ tutorial ]
image: assets/img/posts/webpack4/webpack4.jpg
# featured: false
# hidden: true
tags: [webpack, webpack4, huongdan, guide, react, es6]
---

## Giới thiệu về Webpack:
Ý tưởng nguyên thủy của Webpack là một module bundler, tức là một công cụ đóng gói module, như css, js. Nhưng với việc cộng đồng các developer trên thế giới với những đóng góp không ngừng nghỉ của mình đã làm phai nhạt ranh giới giữa một module bundler và task runner, điển hình như Gulp.

Qua gần 6 năm từ khi xuất hiện, Webpack đã được đón nhận như một công cụ không thể thiếu giúp cho viện phát triển web, mặc dù Webpack không giới hạn ở web. (Deport those who don't use Webpack!!!).

Hiện tại Webpack đang ở phiên bản 4.16.2. Với sự phát triển và thay đổi khá nhanh của mình thì rất nhiều plugin/loader đã bị outdated nhưng không thể phủ nhận sự cải thiện mạnh mẽ của công cụ này.

Hôm nay mình muốn giới thiệu về cách config Webpack 4 từ cơ bản cho đến xây dựng project React cùng ES6 để build production.

------------------------

## Cách sử dụng Webpack 4

Bắt đầu, hãy khởi tạo một vài thứ cần cho bài hướng dẫn này. Mở cmd (terminal) của bạn lên và cd đến thư mục làm việc của bạn, sau đó chạy mấy dòng command sau:

```javascript
mkdir webpack4-awesome
cd webpack4-awesome
```

Khởi tạo package.json
```javascript
npm init -y
```
Cài đặt webpack và webpack-cli (2 package khác nhau nhé)
```javascript
npm i -D webpack webpack-cli
```
Với `-D` (tương đương với `--save-dev`) thì npm sẽ tự động thêm webpack và webpack-cli vào _devDependencies_(trong `package.json`). Còn việc _dependencies_ và _devDependencies_ khác nhau như thế nào các bạn hãy tự tìm hiểu thêm nhé.

-------------------------

#### Webpack + không config
Một trong những điểm mới của Webpack 4 là nó mặc định không cần config!!!

Vậy thử chạy webpack nhá, vào `package.json` thêm một build script như sau:

```javascript
{
    "scripts": {
        "build": "webpack"
    }
}
```
Lưu và chạy thử lệnh sau:
```javascript
npm run build
```
Bạn sẽ nhận được một thông báo lỗi như sau:
```javascript
ERROR in Entry module not found: Error: Can't resolve './src' in 'C:\code4po\webpack4-awesome'
npm ERR! code ELIFECYCLE
npm ERR! errno 2
npm ERR! webpack4-awesome@1.0.0 build: `webpack`
npm ERR! Exit status 2
npm ERR!
npm ERR! Failed at the webpack4-awesome@1.0.0 build script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\po\AppData\Roaming\npm-cache\_logs\2018-07-23T17_19_17_039Z-debug.log
```

Bên cạnh đó là một warning nhè nhẹ như thế này
```javascript
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
```
_Cái này liên quan tới build mode của Webpack 4 nhưng tạm thời mình chưa cần quan tâm đến cái này vội._

Nói về lỗi ở trên thì như bạn có thể đọc thấy, bạn không có file entry (_Entry module not found_), tức là file "đầu vào". Mặc định Webpack sẽ lấy từ `src/index.js`.

Vậy thì mình thử tạo file đó rồi ghi một dòng code đơn giản như là:
```javascript
console.log('Đây là index.js');
```
Sau đó chạy lại 
```javascript
npm run build
```
Nhìn vào thư mục `webpack4-awesome` bạn sẽ thấy một thư mục mới được tạo nên và có 1 file trong đó: `dist/main.js`.

Đúng rồi đấy, bạn cũng không cần phải config thì Webpack mới build ra được file output.

**>>>>> Webpack 4 không cần config file entry và output**

-------------------------

#### Development và Production mode của Webpack 4
Bây giờ hãy nói về cái warning trên:

```javascript
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
```
Đọc sơ qua có thể thấy được là Webpack yêu cầu một đầu vào là _mode_ và nếu như bạn không cung cấp thì Webpack mặc định sẽ để là mode `production`.

Vậy 2 mode này khác nhau như thế nào? Bạn cũng có thể tưởng tượng ra rồi đấy, ở production mode thì file sẽ được compress (minify/uglify) để cho gọn nhẹ dễ deploy. Hãy thử mở file `main.js` được Webpack build ra từ lệnh build khi nãy nhé, nó sẽ có dạng như thế này:
```javascript
!function(e){var t={};function n(r){if(t[r])return t[r].exports;var o=t[r]={i:r,l:!1,exports:{}};return e[r].call(o.exports,o,o.exports,n),o.l=!0,o.exports}n.m=e,n.c=t,n.d=function(e,t,r){n.o(e,t)||Object.defineProperty(e,t,{enumerable:!0,get:r})},n.r=function(e){"undefined"!=typeof Symbol&&Symbol.toStringTag&&Object.defineProperty(e,Symbol.toStringTag,{value:"Module"}),Object.defineProperty(e,"__esModule",{value:!0})},n.t=function(e,t){if(1&t&&(e=n(e)),8&t)return e;if(4&t&&"object"==typeof e&&e&&e.__esModule)return e;var r=Object.create(null);if(n.r(r),Object.defineProperty(r,"default",{enumerable:!0,value:e}),2&t&&"string"!=typeof e)for(var o in e)n.d(r,o,function(t){return e[t]}.bind(null,o));return r},n.n=function(e){var t=e&&e.__esModule?function(){return e.default}:function(){return e};return n.d(t,"a",t),t},n.o=function(e,t){return Object.prototype.hasOwnProperty.call(e,t)},n.p="",n(n.s=0)}([function(e,t){console.log("Đây là index.js")}]);
```

Đúng như bạn đang thấy thì nó chỉ có 1 dòng duy nhất.

Trước đây thì bạn phải dùng một plugin như `uglifyjs` để đạt được điều này, và bây giờ với Webpack 4 thì chức năng này đã được built-in.

Và ngược lại thì `development` mode sẽ giữ nguyên cấu trúc của file cho dễ nhìn. Để Webpack nhận dạng mode nào thì bạn có thể chạy thử 1 trong 2 command sau:
```javascript
webpack --mode development
```
```javascript
webpack --mode production
```
Để override (location mặc định của file entry và output) thì bạn có thể làm như sau:

```javascript
webpack --mode development "path/to/src/index.js" --output "path/to/dist/main.js"
```

```javascript
webpack --mode production "path/to/src/index.js" --output "path/to/dist/main.js"
```

Để chọn tiện sử dụng và bổ sung những đầu vào cần thiết thì bạn hãy thêm 2 script này vào `package.json`

```javascript
{
    "scripts": {
        "build": "webpack --mode production",
        "build:dev": "webpack --mode development"
    }
}
```

Đó là những điểm mới và cơ bản của Webpack 4, nhưng đây vẫn chưa đủ để làm được một project hoàn chỉnh. 

Để Webpack có thể đủ để hỗ trợ cho một project thì phần thiết yếu đó là `plugin` và `loader`. Từ những bước sau chúng ta sẽ được tiếp xúc khá nhiều với những `plugin` và `loader` hay ho và hữu ích.

------------

#### Webpack 4 và ES6
ES6 đã ra được một khoảng thời gian tương đối nhưng các trình duyệt vẫn chưa hỗ trợ đầy đủ để có thể chạy được ES6. 
Vì vậy từ những ngày đầu có ES6, một công cụ đã được sinh ra để _dịch_ (transpile) ES6 thành _Javascript_ mà trình duyệt có thể hiểu được, đó chính là Babel.

![Babel](https://d33wubrfki0l68.cloudfront.net/7a197cfe44548cc1a3f581152af70a3051e11671/78df8/img/babel.svg "Babel")

Vậy Webpack kết hợp Babel như thế nào? Việc này được thông qua một công cụ trung gian, `loader`, chính xác hơn là `babel-loader`.

Cài đặt:

```javascript
npm i -D babel-loader babel-core babel-preset-env
```

Sau đó, bạn hãy tạo một file `webpack.config.js` ở `./webpack.config.js` như sau:
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
};
```

Và tạo file `.babelrc` ở `./.babelrc` như sau:
```javascript
{
    "presets": [
        "env"
    ]
}
```

Hãy nói sơ qua một chút về cách `loader` hoạt động.

Một `loader` chạy như một dạng _middleware_, khi một file bất kỳ được `import` hoặc `require` trong project mà Webpack có đọc qua thì nó sẽ yêu cầu có một `loader` để đọc và xử lý (chuyển đổi, copy,...) những file này. 

Như trên ta có thể thấy một `loader` (_rule_ để dùng `loader`) có 3 phần cơ bản:

`test`: đây là regex để xác định kiểu file (dựa theo tên).

`exclude`: thư mục mà chúng ta muốn Webpack bỏ qua.

`use`: `loader` mà chúng ta muốn Webpack xài để xử lý file.

Và không phải với một `"test"` ta chỉ dùng được một `loader` mà có thể dùng nhiều `loader` cho cùng kiểu file. Ví dụ:

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: [
            {
                loader: "babel-loader"
            },
            {
                loader: "another-awesome-loader"
            }
        ]
      }
    ]
  }
};
```

Và `loader` cũng có thể đi kèm `option`:
```javascript
{
    loader: "babel-loader",
    option: {
        aValidOption: true
    }
}
```

Quay lại ES6 với Webpack + Babel. Sau khi có file `webpack.config.js` thì bạn hãy mở file `./src/index.js` lên và thêm một _arrow function_ của ES6 như sau:

```javascript
const es6Func = str => console.log(str);

es6Func('I am printed by an arrow function');
```

Sau đó chạy 
```javascript
npm run build
```

Hãy kiểm tra `./dist/main.js` để xem Webpack và Babel đã dịch ra sao nhé!   

--------------

#### Webpack và React

Để cho Webpack có thể đọc và dịch được file `jsx` thì bạn phải cần một `loader` như mọi kiểu file khác. 
Nhưng chúng ta không cần phải cài thêm một `loader` nào nữa vì `babel-loader` cũng có thể được việc đó, việc chúng là cần làm nói cho `babel-loader` cần phải xử lý `jsx`.

Chúng ta cần làm 3 thứ:
1. Cài preset `react`
```javascript
npm i -D babel-preset-react
```

2. Thêm preset vào `.babelrc`
```javascript
{
    "presets": [
        "env",
        'react'
    ]
}
```
3. Sửa `webpack.config.js` như sau

```javascript
module.exports = {
  resolve: {
    extensions: ['.js', '.jsx']
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
            loader: "babel-loader"
        }
      }
    ]
  }
};
```

Bây giờ chúng ta cần kiểm tra xem những config này ổn hay không!

Tạo một component React ở `./src/App.jsx`

```javascript
import * as React from 'react';
import ReactDOM from "react-dom";

const App = () => {
    return (
        <div>React Component</div>
    );
};

export default App;

ReactDOM.render(<App />, document.getElementById("root"));
```

Và import vào file entry `./src/index.js`.

```javascript
import App from './App';
```

Đừng quên cài `react` và `react-dom` trước khi chạy lệnh build lại:


```javascript
npm i --save react react-dom
```

Nếu như không có gì sai sót bạn sẽ nhận được 1 thông báo như thế này:

```javascript
$ npm run build

> webpack4-awesome@1.0.0 build C:\code4po\webpack4-awesome
> webpack --mode production

Hash: f1afc79f508fe6d12457
Version: webpack 4.16.2
Time: 733ms
Built at: 2018-07-24 23:24:33
  Asset     Size  Chunks             Chunk Names
main.js  100 KiB       0  [emitted]  main
Entrypoint main = main.js
[5] ./src/index.js 183 bytes {0} [built]
[6] ./src/App.jsx 862 bytes {0} [built]
    + 14 hidden modules
```

Nhưng như vậy chúng ta vẫn chưa _nhìn thấy_ được react component này hoạt động như thế nào. Đó sẽ công việc mà chúng ta sẽ làm trong bước tiếp theo.

--------------

#### Webpack + React và `html-webpack-plugin` + `html-loader`

Tới bây giờ bạn cũng đã có thể hình dung `html-loader` sẽ làm gì rồi! Xử lý `.html`.

Còn `html-webpack-plugin` sẽ làm việc inject (tiêm) một đống thứ như js, css và trong file html, và cũng đảm nhiệm luôn việc đẩy ra file output.

Cài đặt: 

```javascript
npm i -D html-webpack-plugin html-loader
```

Thay đổi `webpack.config.js`

```javascript
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  resolve: {
    extensions: ['.js', '.jsx']
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
};
```

Sau đó tạo file `index.html` ở `./src/index.html`:
```html
<!DOCTYPE html>
<html>
    <head>
        <title>React</title>
    </head>
    <body>
        <div id="root">
        </div>
    </body>
</html>
```

Và chạy lại lệnh build:

```javascript
npm run build
```

Giờ bạn có thể mở thư mục `dist` lên và mở file `index.html` bằng trình duyệt và xem kết quả!

![index.html](/assets/img/posts/webpack4/index.html.png "index.html")

Trong quá trình dev bạn có thể dùng `webpack-dev-server`, một plugin tự động reload(tải lại trang) khi có thay đổi trong project (mà Webpack đang dùng để build).

```javascript
npm i -D webpack-dev-server
```

Chỉnh scripts trong `package.json` lại như sau:
```javascript
{
    "scripts": {
        "start": "webpack-dev-server --mode development --open",
        "build": "webpack --mode production",
        "build:dev": "webpack --mode development"
    }
}
```
Và từ giờ, khi dev, bạn chỉ cần chạy
```javascript
npm start
```

----------------

#### Và bây giờ cần làm gì nữa?

##### Hết rồi!!!

Đúng là hết rồi. Vì nếu đọc tới đây bạn đã cơ bản hiểu được cách config Webpack 4 rồi đó.

Bây giờ khi gặp một kiểu file mới (hoặc cũ) mà bạn muốn xử lý, hãy lên google và tìm một `loader` thích hợp.

Còn nếu như bạn muốn chạy một task nào đó, hãy tìm `plugin` tương ứng.

Đây là kết quả sau khi làm tất cả các bước trong bài: <https://github.com/shortgiraffe4/webpack4-guide>

Nếu bạn vẫn còn cảm thấy bối rối thì hãy thử setup của mình tại: [React starter with Typescript](https://github.com/shortgiraffe4/react-typescript-starter), một boilerplate mình tạo ra để đỡ tốn công setup sau này khi làm dự án React.

Happy coding!!!
