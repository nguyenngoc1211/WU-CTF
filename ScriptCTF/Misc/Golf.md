# Golf

## Tóm tắt
Bài này không giới hạn theo số ký tự mà theo chiều rộng render của source code. Vì vậy lời giải là nhét dữ liệu bằng Unicode có bề rộng rất nhỏ rồi decode khi chạy.

## Hướng giải
- Đọc `server.py` thấy giới hạn thật sự là:

```python
if font.getlength(code) > 380:
    return "TOO LONG"
```

- Server còn render chính source code thành ảnh trước khi chạy, nên đây là giới hạn theo pixel width chứ không phải `len(code)`.
- Output cần in là ma trận xoắn ốc 10x10 chứa các số từ `0` đến `99`.
- Thay vì viết thẳng 100 số, mình encode mỗi giá trị `n` thành `chr(768 + n)` rồi lưu toàn bộ ma trận vào một chuỗi Unicode rất hẹp.
- Khi chạy chỉ việc decode ngược bằng `ord(c) - 768` và in ra theo từng hàng.

```python
s = "..."
for i in range(0, 100, 10):
    print(*[ord(c) - 768 for c in s[i:i+10]])
```

## Kết quả
Payload vượt qua giới hạn hiển thị, in đúng spiral 10x10 và server trả `scriptCTF{...}`.
