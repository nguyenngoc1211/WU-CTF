# Badge Check

## Tóm tắt
File PE không có strings hữu ích, nhưng section `.rsrc` rất lớn. Dữ liệu cần thiết thực ra là một PNG nhúng trong resource và bên trong ảnh có mã PDF417.

## Hướng giải
- Kiểm tra file thấy `badgecheck.exe` là PE64 và phần `.rsrc` chiếm dung lượng lớn, nên khả năng cao có dữ liệu nhúng.
- Trích PNG từ EXE bằng cách tìm signature `\x89PNG` và kết thúc ở `IEND`.
- Mở ảnh ra thấy đây là một badge truy cập có barcode PDF417 ở phần dưới.
- Decode PDF417 bằng công cụ quét barcode là ra ngay text chứa thông tin badge và một trường `FLAG=...`.

```python
from pathlib import Path

data = Path("badgecheck.exe").read_bytes()
start = data.index(b"\x89PNG\r\n\x1a\n")
end = data.index(b"IEND", start) + 8
Path("badge.png").write_bytes(data[start:end])
```

## Kết quả
Giải mã PDF417 từ ảnh nhúng và lấy được `ThryveCTF{...}`.
