# RedLine

## Tóm tắt
Hai chuỗi `ThryveCTF{...}` trong `strings` chỉ là mồi nhử. Binary thực sự kiểm tra một credential dài 36 ký tự bằng một mạch logic với 320 bit đầu ra.

## Hướng giải
- Từ hàm chính, xác định chương trình:
  1. đọc input dài đúng 36 byte;
  2. tách input thành bit;
  3. chạy qua một mạng gate;
  4. so sánh 320 bit output với bảng target.
- Thay vì reverse toàn bộ bằng tay, mình dump ba phần từ binary:
  - bảng gate ở `0x4a14a0`,
  - danh sách output index ở `0x47ec00`,
  - bit target ở `0x47ef20`.
- Mỗi gate ứng với một trong các primitive `NOT`, `XOR`, `NAND`, `COPY`, `NOISE`.
- Mô hình hóa mạng này bằng Z3 với ràng buộc prefix `ThryveCTF{` và ký tự cuối `}`. Gate `NOISE` có thể model như các biến boolean phụ.

```python
for func, a, b, c in gates:
    if func == G_NOT:
        cells[b] = Not(cells[a])
    elif func == G_XOR:
        cells[c] = Xor(cells[a], cells[b])
    elif func == G_NAND:
        cells[c] = Not(And(cells[a], cells[b]))
    elif func == G_COPY:
        cells[b] = cells[a]
        cells[c] = cells[a]
```

## Kết quả
Z3 khôi phục được credential hợp lệ và binary chấp nhận input đó, cho ra flag `ThryveCTF{...}`.
