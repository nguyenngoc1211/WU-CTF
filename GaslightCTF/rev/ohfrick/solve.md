# ohfrick

## Tóm tắt
File Python bị obfuscate rất nặng, nhưng phần logic thật phía sau chỉ là một bộ điều kiện kiểm tra input dài 14 ký tự.

## Hướng giải
- Không chạy `exec` ngay; bóc lớp obfuscation để lấy phần code thật.
- Core checker rút gọn về các điều kiện trên từng vị trí của chuỗi `f`.
- Từ các ràng buộc:
  - `f[0] = 'e'`
  - `f[1:3] = "50"`
  - `"reset" == f[5] + f[0] + f[9] + f[0] + f[3]`
  - `f[8] == '_'`
  - `f[-1] == '3'`
- Giải lần lượt các ký tự còn lại thu được input đúng:

```text
e50t3r1c_sn4k3
```

- Chạy lại file với input này thì chương trình in `ok`.

## Kết quả
Đáp án của checker là `e50t3r1c_sn4k3`; đây là chuỗi cần nộp hoặc dùng tiếp trong challenge.
