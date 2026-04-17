---
description: >-
  A Secure Printer is now in use. I’m confident no one can leak the message
  again... or can you? Two printers are on a given port, one public, one
  private.
---

# Printer Shares 2

Khi thử kết nối bằng `nc` hoặc truy cập web, service không trả về thông tin gì. => không phải là dịch vụ web hay TCP thông thường. Sau khi enumerate bằng `smbclient`, ta xác định đây là SMB service và tiến hành khai thác thông qua các share.

Ta bắt đầu bằng việc liệt kê SMB shares:

```
smbclient -L //green-hill.picoctf.net -p <PORT> -N
```

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Kết quả ra : **shares**
, **secure-shares**
&#x20;và **IPC$**

sau đó truy cập vào **shares:**&#x20;

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

dùng lệnh **ls** để liệt kê các file rồi sau đó **get\<tên file>** để tải file về:

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

và đây là nội dung 3 file:

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

⇒ username được leak là **Joe**

Dựa vào hint:

> rockyou.txt is pretty common for password cracking

ta tiến hành brute-force (chạy 30 luồng cho nhanh):

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

ra được password là **popcorn**

sau đây ta đăng nhập lại vào **secure-shares** bằng mk vừa tìm đc

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

lấy file **flag.txt** ra **cat,** ta sẽ tìm ra được flag

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

