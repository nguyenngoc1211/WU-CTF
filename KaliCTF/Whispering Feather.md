# Whispering Feather

## Tóm tắt
Đây là binary ARM64 có chủ đích thả vài chuỗi `KaliTeam{...}` giả trong `strings`. Input đúng không nằm sẵn ở dạng plaintext; chương trình tự dựng một composite response rồi mới kiểm tra.

## Hướng giải
- `README.txt` đã nói trước rằng flag-shaped strings trong binary chỉ là decoy, nên không thể dừng ở `strings`.
- Điểm bám tốt nhất là prompt `Present the three seals:` và nhánh in `[+] seals aligned; selecting a handler...`.
- Mình debug bằng QEMU user + GDB/IDA, đặt breakpoint ở nhánh pass để xem buffer input và các vùng dữ liệu vừa được chương trình dựng.
- Ở thời điểm pass, buffer chứa sẵn composite response hoàn chỉnh theo dạng:

```text
wing-<base32_like_chunk>:<hex>:<hex>
```

- Trên binary local, chuỗi cần nhập là:

```text
wing-CSBWUGKJGUHGSGJ4F5XB:037413d7:7b456423ebd50c2f
```

- Nhập lại đúng chuỗi này thì binary đi qua toàn bộ validation chain và in flag thật.

## Kết quả
Loại được các decoy trong `strings`, khôi phục input đúng của checker và nhận `KaliTeam{...}`.
