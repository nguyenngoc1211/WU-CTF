# where the dream start 2

## Tóm tắt
Ciphertext của bài là columnar transposition. Chỉ cần brute-force độ dài khóa là đủ để lộ bản rõ.

## Hướng giải
- Đọc `output.txt` và thử giải với các `keylength` từ nhỏ đến khoảng 30.
- Với `keylength = 3`, bản rõ hiện ra rõ ràng: câu `"The flag is ..."` kèm theo chuỗi flag ở giữa.
- Hai ký tự cuối chỉ là padding, nên bỏ đi là xong.

```python
ct = open("output.txt").read().strip()

def decrypt(ct, k):
    rows = (len(ct) + k - 1) // k
    extra = len(ct) % k
    lens = [rows if extra == 0 or i < extra else rows - 1 for i in range(k)]

    cols = []
    pos = 0
    for n in lens:
        cols.append(ct[pos:pos+n])
        pos += n

    return "".join(
        cols[c][r]
        for r in range(rows)
        for c in range(k)
        if r < len(cols[c])
    )

for k in range(2, 30):
    pt = decrypt(ct, k)
    if "CTF{" in pt or "flag" in pt.lower():
        print(k, pt)
```

## Kết quả
`k = 3` cho ra bản rõ hợp lệ và lấy được flag `gaslightCTF{...}`.
