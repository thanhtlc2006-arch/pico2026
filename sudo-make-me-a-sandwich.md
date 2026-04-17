# Sudo make me a sandwich

Đầu tiên truy cập vào dịch vụ và dùng **ls** để ktra nhg ta thấy ko có quyền cơ bản bth

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

ta kiểm tra các quyền **sudo** mà user hiện tại có bằng **sudo -l**

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

thấy dòng&#x20;

```
(ALL) NOPASSWD: /bin/emacs
```

\=>có thể chạy **Emacs với quyền root** mà không cần password (Áp dụng cho **ALL user** → tức là root) luôn

Ta mở **emacs** với **sudo,** mở **shell** trong emacs và **cat** file **flag.txt** ra là được flag

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
