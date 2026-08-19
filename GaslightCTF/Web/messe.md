# messe

## Tóm tắt
Đây là ORDER BY injection kiểu side-channel. API không trả cột `secret`, nhưng vẫn cho sort theo `secret`, từ đó làm lộ thứ tự tương đối của password admin.

## Hướng giải
- Endpoint quan trọng là:

```text
GET /api/stories?column=...&order=...
```

- Server đưa `column` thẳng vào `ORDER BY`, nên có thể gọi:

```text
/api/stories?column=secret&order=ASC
```

- `secret` không hiện trong JSON, nhưng thứ tự các author thay đổi theo giá trị thật của cột này.
- Tạo nhiều tài khoản với password do mình chọn, post story để chúng xuất hiện trong danh sách, rồi dùng vị trí của các user thử nghiệm so với `admin` làm mốc so sánh.
- Dò dần prefix của secret admin theo thứ tự từ điển cho tới khi đủ giá trị, sau đó đăng nhập admin và đọc story `close_friends`.

## Kết quả
Khôi phục được secret của admin và lấy flag từ story riêng tư.

## Gốc lỗi
`ORDER BY` động không whitelist tên cột, trong khi database vẫn chứa cột nhạy cảm là `secret`.
