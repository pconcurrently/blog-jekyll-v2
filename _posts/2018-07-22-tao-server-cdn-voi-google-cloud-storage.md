---
layout: post
title:  "Tạo server CDN với Google Cloud Storage và domain tùy chọn"
author: po
# categories: [ tutorial ]
image: assets/img/posts/cdn/cdn.png
# featured: false
# hidden: true
tags: [cloudflare, cdn, google-cloud, google-storage, guide]
---

## CDN là gì?
CDN là viết tắt của Content Delivery Network. Nói một cách đơn giản thì đây là nơi lưu trữ các file (các static asset) rồi được sử dụng bởi nhiều người dùng khác nhau. Bạn có thể google để biết thêm, còn đây là cách bạn có thể tạo cho mình một server CDN để host các file của mình.

## Tạo server CDN đơn giản với Google Cloud Storage

##### 1. Đăng ký sử dụng miễn phí 1 năm Google Cloud với $300 credit

Truy cập vào trang: <https://console.cloud.google.com/freetrial> để đăng ký với tài khoản Google của bạn.

![Google Cloud Free trial](/assets/img/posts/cdn/gc-freetrial.png "Google Cloud Free trial")

Sau khi đăng ký thành công thì chúng ta đã có $300 để tiêu xài hoang phí rồi, nếu có vấn đề gì có thể liên hệ phụ huynh để giải quyết.

##### 2. Tạo bucket trên Google Storage

Sau đó để làm những bước tiếp theo, hãy tạo cho mình một project trên Google Cloud. Sau khi tạo xong hãy tiếp tục.

Vào <https://console.cloud.google.com/storage/> hoặc trực tiếp bấm vào menu phía bên trái kéo xuống và chọn Storage:

![Google Cloud Free trial](/assets/img/posts/cdn/gs-menu.png "Google Cloud Free trial")

Tiếp theo chọn *Create a bucket*. Điền thông tin như hình. 
**\* Lưu ý tên của bucket cũng chính là tên sẽ dùng cho domain. Vì Google Storage không cung cấp cho người dùng cách nào để mapping với custom domain.**
Ở đây mình dự định sẽ tạo CDN với URL là <https://cdn.phohuynh.com/>

![Create bucket](/assets/img/posts/cdn/create-bucket.png "Create bucket")

##### 2. Public bucket

Để (nguời dùng) access được file trên CDN tạo bở Goolge Storage thì chúng ta phải public những file này.

Hãy vào <https://console.cloud.google.com/storage/browser> sau đó bấm vào nút _ba chấm_ phía bên phải của bucket. Chọn _Edit bucket permission_ rồi điền thông tin như hình rồi bấm _Add_

![Edit permission](/assets/img/posts/cdn/edit-permission.png "Edit permission")

##### 3. Mapping với domain

Bước thứ 3 này bạn cần phải sở hữu một domain, bạn có thể dễ dàng mua được một domain với giá khá rẻ trên <https://vn.godaddy.com/> . Bạn có thể lựa chọn những dịch vụ khác như <https://domains.google/> nhưng mình thấy Godaddy có giá khá ổn và có nhiều khuyến mãi nữa.

Nếu bạn đã sở hữu một domain rồi thì hãy vào phấn cấu hình của domain, chọn _DNS setiings_, tạo một _CNAME_ record như sau:
```javascript
    value: cdn
    target: c.storage.googleapis.com
```
Trên website quản lý domain thì mọi thứ nhìn sẽ tương tự như thế này:

![CNAME](/assets/img/posts/cdn/cname-cdn.png "CNAME")

Tùy dịch vụ cung cấp DNS mà có thể sau vài giây, vài phút hoặc vài giờ sau record sẽ được cập nhật. Khi thành công thì bucket của bạn sẽ có thể truy cập được từ địa chỉ, tương tự như <http://cdn.phohuynh.com>. Lưu ý là _http_ chứ chưa phải là _https_. Thực ra mình đang sử dụng dịch vụ Cloudflare để quản lý domain mình mua trên Godaddy nên mình có được SSL miễn phí và địa chỉ CDN của mình sẽ là <https://cdn.phohuynh.com>.

##### 4. Upload file và dùng thử

Vào <https://console.cloud.google.com/storage/browser/> sau đó bấm vào bucket bạn đã tạo.

![Upload to bucket](/assets/img/posts/cdn/upload-bucket.png "Upload to bucket")

Bạn có thể chọn một file nào đó để upload. Mình sẽ upload thử file _bootstrap.min.css_ để test.

![Upload to bucket](/assets/img/posts/cdn/bucket-file.png "Upload to bucket")

Sau khi upload lên và bạn thấy cột _Share publicly_ là _Public link_ thì bạn đã thành công upload lên CDN của mình rồi đó.

Bây giờ thử access file này bằng cách truy cập vào link, tương tự như: _http://cdn.domaincuaban.com/bootstrap.min.css_

Của mình là: <https://cdn.phohuynh.com/bootstrap.min.css>

Bạn có thể tạo các thư mục khác nhau để sắp xếp các file này. Bạn còn có thể dùng công SDK của Google Cloud rồi dùng CLI để upload một thư mục (project) lên Google Storage. Bạn có thể tìm hiểu thêm tại đây: <https://cloud.google.com/sdk/> hoặc đợi một bài khác từ blog của mình :v

#### 5. Tổng kết

Bây giờ bạn đã có CDN server đơn giản để host các file static một cách hơi cool ngầu hơn rồi hê hê
