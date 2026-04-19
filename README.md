# Forensics Git 0

***

1\. Mô tả bài toán

Bài cho một file disk.img và yêu cầu tìm flag.\
Hint: _“How can you checkout the files of a previous commit?”_

→ Gợi ý rất rõ: flag nằm trong Git repository và có thể ở commit cũ.

***

2\. Phân tích disk image

Kiểm tra partition:

mmls disk.img

Kết quả:

· Partition Linux tại offset 1140736 (đáng chú ý)

***

3\. Duyệt filesystem

Liệt kê file:

fls -o 1140736 -r disk.img

Tìm được đường dẫn đáng nghi:

/home/ctf-player/Code/secrets/.git

→ Đây là Git repository, rất phù hợp với hint.

***

4\. Phân tích thư mục .git

Liệt kê nội dung:

fls -o 1140736 -r disk.img 65665

Quan sát thấy:

· refs/heads/master

· objects/

· HEAD

→ Repo Git đầy đủ → có thể extract commit.

***

5\. Lấy commit hiện tại

Đọc file master:

icat -o 1140736 disk.img 65702

Sau đó tìm object tương ứng trong:

.git/objects/

***

6\. Giải nén Git object

Ví dụ commit object:

icat -o 1140736 disk.img 65700 | python3 -c "import zlib,sys;print(zlib.decompress(sys.stdin.buffer.read()))"

Kết quả:

commit ...

tree ...

author ...

committer ...

&#x20;

Wrap this phrase in the flag format: g17\_1n\_7h3\_d15k\_041217d8

***

7\. Hiểu nội dung

Ngoài ra trong repo còn có hint:

The picoCTF flag format is 'picoCTF{}'

→ Nghĩa là:

· Flag = picoCTF{...}

· Nội dung bên trong là leetspeak phrase

***

8\. Suy ra flag

Phrase tìm được:

g17\_1n\_7h3\_d15k\_041217d8

→ Flag:

picoCTF{g17\_1n\_7h3\_d15k\_041217d8}

***

9\. Kết luận

· Bài này yêu cầu:

o Phân tích disk image bằng The Sleuth Kit

o Tìm Git repository

o Trích xuất commit object

o Đọc nội dung commit để lấy flag

· Điểm mấu chốt:

Flag không nằm trong file hiện tại mà nằm trong lịch sử commit (previous commit)

***

✅ Flag

picoCTF{g17\_1n\_7h3\_d15k\_041217d8}

&#x20;
