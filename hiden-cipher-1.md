# Hiden Cipher 1

Đầu tiên đề cho 1 file .zip, ta tiến hành giải nén nó ra&#x20;

được file binary là **hiddencipher** và file fake flag **flag.txt** :))

dùng lệnh **strings** để có thể đọc được các kí tự có thể đọc được trong bài

ta thấy được nhiều chuỗi lạ và mấu chốt là phần info này

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

từ đây ta tiếp tục giải nén bằng UPX

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

sau khi giải nén ta **strings**  thấy nhiều thông tin rõ ràng hơn:

* Có lời gọi tới `flag.txt` → chương trình sẽ cố mở file này.
* Có thông báo **“Here your encrypted flag:”** → nghĩa là nội dung trong `flag.txt` đã bị mã hóa.
* Có hàm `get_secret` trong binary → nhiều khả năng đây là nơi thực hiện phép XOR để mã hóa/giải mã.

và **objdump -d** lại xem như nào ta thấy trong hàm **get\_secret** có

```
12b1: c6 05 ... 53    movb   $0x53,0x4011 <s.0>
12b8: c6 05 ... 33    movb   $0x33,0x4012 <s.0+0x1>
12bf: c6 05 ... 43    movb   $0x43,0x4013 <s.0+0x2>
12c6: c6 05 ... 72    movb   $0x72,0x4014 <s.0+0x3>
12cd: c6 05 ... 33    movb   $0x33,0x4015 <s.0+0x4>
12d4: c6 05 ... 74    movb   $0x74,0x4016 <s.0+0x5>
```

Các giá trị hex này tương ứng với ASCII:

* `0x53` → `'S'`
* `0x33` → `'3'`
* `0x43` → `'C'`
* `0x72` → `'r'`
* `0x33` → `'3'`
* `0x74` → `'t'`

⇒ key là **S3Cr3t**



ta truy cập vào dịch vụ và nhận được chuỗi hex&#x20;

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

dựa vào hint ta đi XOR với key vừa tìm đc bên trên<br>

```python
import binascii

hex_data = "235a201d702015483b1d412b265d3313501f0c072d135f0d2002302d06476350224507462e"
data = binascii.unhexlify(hex_data)

key = b"S3Cr3t"
decrypted = bytes([b ^ key[i % len(key)] for i, b in enumerate(data)])
print(decrypted.decode())
```



⇒ Flag:

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
