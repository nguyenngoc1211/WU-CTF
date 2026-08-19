# blackout

## Tóm tắt
Flag không bị mã hóa, chỉ bị ẩn trong PDF sau khi recover file.

## Hướng giải
- Trích text từ file recovered:

```bash
pdftotext -raw recovered_file out.txt
```

- Tìm theo format flag:

```bash
grep -ao 'gaslightCTF{[^}]*}' out.txt
```

## Kết quả
Lấy được flag `gaslightCTF{...}` trực tiếp từ text đã trích.
