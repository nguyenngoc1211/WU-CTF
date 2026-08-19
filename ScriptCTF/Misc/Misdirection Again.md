# Misdirection Again

## Tóm tắt
Chuỗi đầu vào nhìn như binary ASCII, nhưng độ dài không chia hết cho 8. Cách đúng là coi toàn bộ nó như một số nhị phân rất lớn.

## Hướng giải
- Thử tách 8 bit ra ASCII không cho kết quả hợp lý.
- Vì chuỗi không phải bội của 8, mình đổi toàn bộ bitstring thành một số nguyên:

```python
bits = "..."
decimal = str(int(bits, 2))
```

- Chuỗi decimal nhận được thực chất là các mã ASCII nối liền nhau.
- Tách greedily thành các số 2 hoặc 3 chữ số nằm trong khoảng printable rồi chuyển sang ký tự.

```python
result = ""
i = 0

while i < len(decimal):
    if i + 3 <= len(decimal) and 32 <= int(decimal[i:i+3]) <= 126:
        result += chr(int(decimal[i:i+3]))
        i += 3
    else:
        result += chr(int(decimal[i:i+2]))
        i += 2

print(result)
```

## Kết quả
Khôi phục được chuỗi `scriptCTF{...}` theo pipeline `binary -> integer -> decimal ASCII -> text`.
