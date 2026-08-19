# 404 Found

## Tóm tắt
Tên bài đánh lạc hướng sang shopping cart, nhưng điểm đáng xem nhất lại là `robots.txt`.

## Hướng giải
- Ban đầu mình kiểm tra cart, checkout và promo code nhưng không thấy gì đặc biệt.
- Mở `robots.txt` thì thấy:

```text
User-agent: *
Disallow: /the-best-robot
```

- Thử truy cập thẳng endpoint bị disallow:

```http
GET /the-best-robot
```

- Server trả về flag trực tiếp trong response body.

## Kết quả
Không cần đụng vào cart logic; chỉ cần đọc `robots.txt` rồi vào endpoint ẩn để lấy `scriptCTF{...}`.
