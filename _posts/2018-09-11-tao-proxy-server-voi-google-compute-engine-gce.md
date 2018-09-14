---
layout: post
title:  "Tạo proxy server với Google Compute Engine (GCE)"
author: po
# categories: [ tutorial ]
image: assets/img/posts/proxy-gce/proxy-3.png
# featured: false
# hidden: true
tags: [proxy, google-compute-engine, google-cloud, guide]
---

## Proxy là gì?
Nếu bạn đọc bài này thì chắc cũng không cần phải được giải thích về proxy nữa. Mình sẽ hướng dẫn cách tạo một proxy server rồi dùng trên trình duyệt web với `squid` và `Google Compute Engine` của Google Cloud.

## Chuẩn bị.
Bạn đã có tài khoản Google Cloud và đã setup billing, Google vẫn đang cung cấp gói dùng thử có thể tham khảo cách tạo tại đây:

[Tạo tài khoản Google Cloud $300](https://blog.phohuynh.com/2018/07/22/tao-server-cdn-voi-google-cloud-storage.html#t%E1%BA%A1o-server-cdn-%C4%91%C6%A1n-gi%E1%BA%A3n-v%E1%BB%9Bi-google-cloud-storage)


## Bước 1: tạo một máy ảo (VM) bằng Google Compute Engine
Vào <https://console.cloud.google.com/compute/instances>, bấm nút "Create Instance" để tạo VM.

![Create Instance](/assets/img/posts/proxy-gce/create-instance.png "Create Instance")

Sau đó cứ để các option mặc định và đổi Boot Disk sang Ubuntu 18.04 LTS, giảm memory xuống 1GB (thấp nhất), chõ đỡ tốn tiền, vì mình cũng không cần máy ảo mạnh lắm cho task này:

_Lưu ý là "Region" sẽ là khu vực của proxy mình tạo, hiện tại đang là Mỹ._

![Create VM](/assets/img/posts/proxy-gce/create-vm.png "Create VM")

## Cài đặt Squid trên VM

Sau khi tạo thành công VM, bấm vào nút "SSH" để mở cửa số command line của VM trên cửa sổ mới. Cửa sổ này sẽ trông như thế này:

![VM SSH](/assets/img/posts/proxy-gce/vm-ssh.png "VM SSH")

Bây giờ mình đã có thể cài đặt Squid:

Chạy lần lượt các command sau (chọn Y, enter nếu nó hỏi):

```python
sudo apt update
sudo apt install squid
sudo apt-get install apache2-utils
```

Sau khi chạy xong, xóa file config mặc định của squid và tạo một file mới:

```python
sudo rm /etc/squid/squid.conf
sudo vi /etc/squid/squid.conf
```
Dán dòng sau vào VIM khi tạo file config mới (sau khi bấm phím A để insert):

```python
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwords
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED
http_access allow authenticated
# Choose the port you want. Default: 3128.
http_port 8888
```
Lưu bằng cách bấm lần lượt `Esc` -> `Shift ;` -> `x` -> `Enter`

Tạo tải khoản để truy cập proxy:

```python
sudo htpasswd -c /etc/squid/passwords [tai_khoan]
```

Ví dụ:

```python
sudo htpasswd -c /etc/squid/passwords po
```
Gõ password 2 lần, Enter.

## Tạo firewall rule để chấp nhận port 8888

Bấm vào dấu ba chấm (phía bên phải) -> View network details để config firewall rule.

![Network detail](/assets/img/posts/proxy-gce/vm-network-detail.png "Network detail")

Bấm vào Firewall rules phía bên trái -> Create Firewall Rule

![Firewall rules menu](/assets/img/posts/proxy-gce/firewall-rules-menu.png "Firewall rules menu")

Điền các trường như sau rồi bấm Create:

![Firewall rule config](/assets/img/posts/proxy-gce/firewall-rule-config.png "Firewall rule config")

## Chạy squid service và sử dụng proxy

Bây giờ mình sẽ chạy squid bằng:
```python
sudo systemctl restart squid.service
```

_Lưu ý: Google sẽ charge phí chạy VM theo giờ nên khi bạn không sử dụng proxy hãy tắt VM đi_

Setup đã xong, bây giờ mình chỉ cần lấy địa chỉ IP (external) của VM rồi dùng thôi:

Vào <https://console.cloud.google.com/compute/instances> và tìm VM mình đã setup, copy địa chỉ IP nó ra:

![VM IP](/assets/img/posts/proxy-gce/vm-ip-2.png "VM IP")

Ở đây, IP của VM mình tạo là `35.231.58.151`. Bây giờ mình sẽ xài thử proxy này trên Firefox, với cài đặt như sau:

![Firefox proxy settings](/assets/img/posts/proxy-gce/firefox-proxy-settings.png "Firefox proxy settings")

Sau đó lưu lại, truy cập vào một trang bất kỳ, nhập thông tin user bạn đã tạo vào, OK.

![Auth](/assets/img/posts/proxy-gce/basic-auth-proxy.png "Auth")

Bây giờ bạn đã có thể sử dụng Firefox với proxy này rồi. Bạn có thể truy cập vào <https://www.whatismyip.com/> để kiểm tra:

![Whatismyip](/assets/img/posts/proxy-gce/whatismyip.png "Whatismyip")

Mình phải nhắc lại một lần nữa vì nếu để VM chạy liên tục kể cả khi không sử dụng thì một tháng bạn có thể bỏ ra khoảng $20 đó, nên hãy tắt nó đi sau khi dùng xong (Bấm vào Stop phía menu bên phải).


#### Credits

Tham khảo: <http://blog.antbytes.com/2016/11/28/how-to-set-up-a-proxy-server-on-google-compute-gce/>

Hình: <http://hackers-workshop.net/hunting-open-proxy-servers-with-kali-linux>
