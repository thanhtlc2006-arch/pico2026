# Gatekeeper

Đầu tiên chúng ta xem xét các thông tin căn bản của file

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

ta thử dùng **strings** để đọc các kí tự có thể đọc trong file đề bài và thấy các điểm chú í

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

⇒ có 2 hàm **is\_valid\_hex** và **is\_valid\_decimal** check

ta thử truy cập dịch vụ nhập 1 vài input

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

⇒ có điều kiện khác

ta thử dùng **objdump -d** để xem xét kĩ hơn và  ta thấy rõ logic trong `main`:

* Input được đọc bằng `scanf` vào buffer.
* Chương trình kiểm tra:
  1. Nếu là **decimal** → dùng `atoi`.
  2. Nếu là **hex** → dùng `strtol` với base 16.
  3. Nếu không hợp lệ → “Invalid input”.
* Sau đó so sánh giá trị:
  * Nếu ≤ 999 (`0x3e7`) → “Too small.”
  * Nếu > 999 nhưng ≤ 9999 (`0x270f`) → tiếp tục.
  * Nếu > 9999 → “Too high.”
* Tiếp theo, nó kiểm tra **độ dài chuỗi nhập** (`strlen`). Nếu đúng bằng 3 ký tự → gọi `reveal_flag`. Nếu không → “Access Denied.”

-> Hướng: nhập một số decimal hoặc hex trong khoảng 1000-9999 và có 3 kí tự

ta thử nhập fff (hex = 4095)

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

ra đc chuỗi này :)) và giờ ta đi xử lí chúng&#x20;

```
s = "}4cdftc_oc_ip6f47ftc_oc_ipa_99ftc_oc_ip9_TGftc_oc_ip_xehftc_oc_ip_tigftc_oc_ipid_3ftc_oc_ip{FTCftc_oc_ipocipftc_oc_ip"
rev = s[::-1]
clean = rev.replace("pi_co_ctf","").replace("ftc_oc_ip","")
print(clean)
```

ra được flag:&#x20;

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>
