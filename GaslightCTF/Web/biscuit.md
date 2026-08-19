# biscuit

## Tóm tắt
Đây không phải bài cookie forgery thông thường mà là Biscuit token injection. Server nhúng thẳng `username` vào policy bằng f-string rồi tự ký token cho mình.

## Hướng giải
- Trang chủ không set session, còn `/flag` tồn tại nhưng yêu cầu session hợp lệ. Vậy hướng đúng là đăng ký tài khoản rồi leo quyền.
- Đọc source thấy token được mint như sau:

```python
builder = BiscuitBuilder(f'''
user("{username}");
check if user($u), $u.length() > 0;
''')
if username == "webmaster":
    builder.add_fact(Fact('role("admin")'))
```

- `username` đi thẳng vào policy, nên có thể thoát khỏi chuỗi `user("...")` và chèn thêm fact mới.
- Payload đăng ký:

```text
ngoc");role("admin");user("x
```

- Sau khi `/signup` trả về cookie `biscuit=...`, chỉ cần dùng lại cookie đó để vào `/flag`. Token lúc này đã chứa `role("admin")` do chính server ký.

## Kết quả
Truy cập được staff room và lấy flag `gaslightCTF{...}`.

## Gốc lỗi
Server tin trực tiếp dữ liệu người dùng khi dựng Biscuit policy. Đây là injection vào auth token, không phải lỗi ký token hay lộ private key.
