# The Add/On Trap

Bài cho file **suspicious.zip**, ta tiến hành giải nén nó ra với mk được cung cấp là **picoctf** và được 1 file .xpi (thực chất chính là **một gói extension của Firefox** – về bản chất nó chỉ là một file .zip được đổi đuôi)

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

ta tiếp tục tiến hành giải nén file đấy và lưu vào thư mục tự tạo là **add\_on\_trap**

<figure><img src=".gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

cấu trúc extension Firefox:

* `manifest.json` → mô tả extension, cho biết script nào chạy.
* `popup.html`, `assets/styles.css` → giao diện.
* `assets/script.js`, `background/main.js` → logic chính, đây thường là nơi ẩn flag hoặc key.
* `META-INF/...` → chữ ký số, không quan trọng cho việc tìm flag.

ta mở file **manifest.json** ra

<figure><img src=".gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

thấy extension này chạy file `background/main.js` khi hoạt động

tiếp tục mở file đấy ra

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

ta thấy được luôn cả **key** và **ciphertext**

```
const key="cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0="
```

```
const webhookUrl='gAAAAABmfRjwFKUB-X3GBBqaN1tZYcPg5oLJVJ5XQHFogEgcRSxSis1e4qwicAKohmjqaD-QG8DIN5ie3uijCVAe3xiYmoEHlxATWUP3DC97R00Cgkw4f3HZKsP5xHewOqVPH8ap9FbE'
```

thấy dấu hiệu trông giống một **Fernet token** (chuỗi bắt đầu bằng **gAAAAA...**).

⇒ ta decode **key** từ base64 và dùng key này với thư viện **cryptography.fernet** để giải mã token

```python
from cryptography.fernet import Fernet

key = b"cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0="
f = Fernet(key)
token = b"gAAAAABmfRjwFKUB-X3GBBqaN1tZYcPg5oLJVJ5XQHFogEgcRSxSis1e4qwicAKohmjqaD-QG8DIN5ie3uijCVAe3xiYmoEHlxATWUP3DC97R00Cgkw4f3HZKsP5xHewOqVPH8ap9FbE"
print(f.decrypt(token).decode())
```

⇒ Flag:

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>
