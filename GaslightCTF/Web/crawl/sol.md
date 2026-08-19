# crawl (302)

## Tóm tắt
Điểm mấu chốt nằm ngay trong `robots.txt`: muốn đóng vai crawler/LLM agent thì phải gửi thêm header `X-LLM-Agent`. Sau đó chỉ cần đi vào thư mục bị disallow.

## Hướng giải
- Mở `robots.txt` và thấy hai chi tiết quan trọng:
  - `Disallow: /super_secret/`
  - ghi chú yêu cầu header `X-LLM-Agent`
- Gửi request với cả `User-Agent` lẫn `X-LLM-Agent`:

```bash
curl -i -A 'GPTBot' -H 'X-LLM-Agent: GPTBot' "$URL/super_secret/"
```

- Server trả directory listing và lộ file `_flag.txt`.
- Lấy file đó bằng cùng bộ header. Không follow redirect để còn quan sát `Location` nếu instance trả 302:

```bash
curl -i -A 'GPTBot' -H 'X-LLM-Agent: GPTBot' "$URL/super_secret/_flag.txt"
```

## Kết quả
Flag xuất hiện trong response trực tiếp hoặc trong header redirect, tùy cách server trả về trên instance đang chạy.
