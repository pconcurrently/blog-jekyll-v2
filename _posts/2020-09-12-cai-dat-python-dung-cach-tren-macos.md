---
layout: post
title:  "Cài đặt Python đúng cách trên MacOS"
author: po
# categories: [ tutorial ]
# featured: true
image: assets/img/posts/default.png
# hidden: true
tags: [python3, macos, python, install-python]
---

## Giới thiệu
MacOS đi kèm với Python (thường là ~`2.7`), nhưng khi code với Python thì phiên bản thông dụng là `3` (`3.8.5` tại thời điểm viết bài). 

Việc cài đặt v3 của Python và sử dụng cùng lúc với v2 không phải là một vấn đề dễ dàng. (Kể cả việc thay thế v2 mặc định của MacOS cũng vậy). Sau nhiều lần vật lộn với Python, thì mình (có vẻ như) tìm được cách tương đối ổn thỏa. Sau đây là cách đó!

## pyenv
```terminal
brew install pyenv
```

(Nếu chưa có `homebrew` thì cài đặt [tại đây](https://brew.sh))

Sau đó chạy lệnh sau để cài đặt `Python 3.8.5`:
```terminal
pyenv install 3.8.5
```

Biến `Python 3.8.5` thành mặc định:

```terminal
pyenv global 3.8.5
```

Bây giờ chúng ta cần để cho `pyenv` kiểm soát version python mỗi lần shell (terminal) của MacOS chạy!
```terminal
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

Thay `.zshrc` bằng `.bash_profile` nếu như bạn không dùng `zsh`.

Reset shell bằng:
```terminal
exec $0
```

Hoặc tắt rồi mở lại một instance mới. Check version shell bằng:

```terminal
which python
```

Happy Coding!
