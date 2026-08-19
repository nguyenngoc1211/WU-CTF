# Wpm-game

## Tóm tắt
Source dùng `eval(wpm.lower())`, vì thế đây là Python expression injection. Blacklist chặn dấu nháy, dấu chấm và `_`, nên payload phải tự dựng chuỗi theo cách khác.

## Hướng giải
- Đọc source và thấy đoạn nguy hiểm:

```python
return jsonify(verdict=rate(eval(wpm.lower())), wpm=float(wpm))
```

- Vì không dùng được quote, mình dựng đường dẫn bằng `bytes([...])` với từng byte viết ở dạng nhị phân.
- Mục tiêu là đọc `/app/flag.txt`, lấy dòng đầu, rồi ném nó vào `open()` lần nữa để ép Flask trả traceback chứa chính chuỗi flag:

```python
open(next(open(bytes([
    ...
]))))
```

- Lần thử đầu với `flag.txt` cho `FileNotFoundError`, từ đó biết file nằm ở chỗ khác.
- Đổi sang `/app/flag.txt` thì traceback trả về đã chứa luôn nội dung cần lấy.

## Kết quả
Khai thác được `eval()` và leak flag `scriptCTF{...}` qua thông báo lỗi.
