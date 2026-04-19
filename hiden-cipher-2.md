# Hiden Cipher 2

Đầu tiên đề cho 1 file .zip, ta tiến hành giải nén nó ra (giống bài 1)&#x20;

được file binary là **hiddencipher2** và file fake flag **flag.txt** :))

dùng lệnh **strings** và ta thấy trong binary `hiddencipher2` có nhiều thông tin quan trọng:

* Có hàm `generate_math_question` → chương trình sẽ hỏi bạn một phép toán.
* Có hàm `encode_flag` → sau khi trả lời đúng, chương trình sẽ lấy flag từ `flag.txt` rồi biến đổi nó bằng một phép toán (thường là XOR hoặc cộng/trừ với đáp số).
* Có chuỗi **“Encoded flag values:”** → tức là chương trình sẽ in ra flag đã bị mã hóa

ta thử truy cập dịch vụ trước và đúng thế&#x20;

<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

⇒ Hướng:&#x20;

* **Đáp số toán** khi nhập vào sẽ đc dùng như một **key** để encode/decode flag.
* **Encoded flag values** chính là ciphertext => đảo ngược phép toán để lấy flag thật

dựa bên trên, ta chia từng số cho 8 để ra mã ASCII gốc r chuyển thành kí tự

```python
values = [896, 840, 792, 888, 536, 672, 560, 984, 872, 416, 928, 832, 760, 784,
          408, 832, 392, 880, 800, 760, 792, 392, 896, 832, 408, 912, 760, 816,
          448, 792, 808, 440, 776, 800, 432, 1000]

key = 8
flag = ''.join(chr(v // key) for v in values)
print(flag)
```

⇒ Flag:

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

