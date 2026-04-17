# Piece by piece

Đầu tiên truy cập vào dịch vụ bằng SSH

dùng **ls** để liệt kê các file và ta đọc được hint từ file **instructions.txt**

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

ta gộp các file lại vào thành 1 file&#x20;

```
cat part_* > full.zip
```

sau đó giải nén nó ra, khi nó bắt nhập mk hãy lấy mk là **supersecret** từ hint và nó giải nén ra file **flag.txt**

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

**cat** ra là ta lấy được flag

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
