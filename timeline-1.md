# Timeline 1

1\. Mô tả bài toán

Bài cho một file disk image partition4.img và yêu cầu:

· Tạo MAC timeline bằng Sleuthkit

· Tìm các file mới tạo (dựa trên timestamp)

· Chú ý dấu hiệu anti-forensics&#x20;

· Lấy flag

***

2\. Khảo sát ban đầu

Kiểm tra file:

ls -lh partition4.img

→ File có dung lượng \~467MB ⇒ hợp lệ

***

3\. Kiểm tra partition

mmls partition4.img

→ Không có output ⇒ image không có partition table

➡️ Kết luận: filesystem nằm trực tiếp trong image (offset = 0)

***

4\. Liệt kê filesystem

fls partition4.img

Kết quả cho thấy cấu trúc Linux:

/home\
/etc\
/root\
/var\
...

***

5\. Tạo MAC timeline

Tạo body file:

fls -r -m / partition4.img > body.txt

Kiểm tra:

wc -l body.txt

→ Có \~8000 dòng ⇒ dữ liệu OK

Tạo timeline:

mactime -b body.txt > timeline.txt

***

6\. Phân tích timestamp

Theo hint đề bài, lọc các file có trạng thái macb:

grep macb timeline.txt

Phát hiện dòng đáng chú ý:

49 macb r/rrw-r--r-- ... /etc/chat

➡️ Đây là file:

· Được tạo/sửa gần thời điểm nghi vấn

· Nằm trong /etc (bất thường)

→ Rất có thể là file chứa flag

***

7\. Trích xuất file nghi vấn

Lấy inode từ timeline:

/etc/chat → inode = 32716

Trích xuất:

icat partition4.img 32716

Output:

NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK

***

8\. Decode dữ liệu

Chuỗi trên là Base64, decode:

echo NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK | base64 -d

Kết quả:

573417h13r\_7h4n\_7h3\_1457\_58527bb222

***

9\. Flag

picoCTF{573417h13r\_7h4n\_7h3\_1457\_58527bb222}

&#x20;
