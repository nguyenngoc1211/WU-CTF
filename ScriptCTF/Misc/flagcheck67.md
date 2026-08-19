# flagcheck67

## Tóm tắt
`pow-solver.py` chỉ là Proof-of-Work. Phần challenge thật nằm ở `check.py`: input chỉ gồm `6` và `7`, sau đó bị ép sang `float` rồi đem đi `%`, `//` và so sánh với các số rất lớn.

## Hướng giải
- Tách riêng từng điều kiện trong `if` để debug thay vì đọc nguyên một biểu thức dài.
- Điểm quan trọng nhất là:

```python
num = float(inp)
```

- Vì `num` là float lớn, các phép toán như `%`, `//` và `==` không còn cư xử như với integer chính xác tuyệt đối.
- Cách làm thực tế là:
  1. giữ nguyên checker local nhưng in từng condition ra riêng;
  2. brute-force các chuỗi chỉ gồm `6` và `7` trong khoảng độ dài hợp lệ;
  3. lọc những giá trị làm các nhánh tất định trở thành `False`;
  4. dùng giá trị đó trên remote sau khi vượt POW.
- Nhánh `random.randint(...) % num` khiến bài này trông có vẻ ngẫu nhiên, nhưng bản chất vẫn là một trò lợi dụng floating-point precision chứ không phải brute-force mù.
- Bản local in `scriptCTF{fakeflag6767}`, còn server thật trả flag ở nhánh thành công.

## Kết quả
Bài này là puzzle về sai số floating-point. Sau khi tìm được input phù hợp và vượt POW, remote trả `scriptCTF{...}`.
