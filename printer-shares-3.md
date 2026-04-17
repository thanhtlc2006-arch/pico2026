---
description: >-
  I accidentally left the debug script in place… Well, I think that's fine - No
  one could possibly access my super secure directory Additional details will be
  available after launching your challenge in
---

# Printer Shares 3

Hint:

* a suspicious script is running every minute
* this script runs every minute, you might need to wait for a while





Sau khi thử netcat cũng như truy cập web mà services ko trả về gì cả, ta enumerate bằng SMB như bài 2 và được kết quả:

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

truy cập vào **shares**, **ls** để liệt kê file và **get** để tải file về

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Nội dung 2 file vừa tải:

<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

ta có thể hiểu rằng file **script.sh** là file được chạy mỗi phút và output sẽ được lưu vào trong file **cron.log,** mà file **script.sh** được lưu trong **shares** có quyền ghi nên ta có thể chỉnh sửa file này

ta tạo script để tìm ra flag

<figure><img src=".gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

sau đó **put** lại để upload cái file vừa sửa vào lại **shares**. dựa vào hint ta cần đợi khoảng 1p để nó ghi lại log và tải lại file **cron.log** về

<figure><img src=".gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

và ta nhìn thấy **/challenge/secure-shares/flag.txt**

ta sửa lại file **script.sh** để đọc đc flag&#x20;

```
echo 'cat /challenge/secure-shares/flag.txt' > script.sh
```

upload lại và chờ 1p, rồi tải lại file **cron.log** về,  **cat** file ra và ta được flag

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>
