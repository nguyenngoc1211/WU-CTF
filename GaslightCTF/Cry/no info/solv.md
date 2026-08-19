# no info

## Tóm tắt
Hint của bài trỏ khá rõ tới "security through obscurity". Ciphertext giữ nguyên độ dài từ và khoảng trắng, nên hướng hợp lý nhất là monoalphabetic substitution.

## Hướng giải
- Điểm bám tốt nhất là dòng cuối: `0w5exk1zn-41fz-f0z-l3exkozn`.
- Từ hint, có thể đoán đây là một câu khẩu hiệu quen thuộc về obscurity/security. Chỉ cần đoán đúng vài ký tự đầu là suy ra được các ánh xạ như `w -> b`, `e -> c`, `x -> u`, `k -> r`, `z -> t`, `n -> y`, `l -> s`, `o -> i`.
- Thế dần các ký tự vào đoạn văn còn lại thì plaintext lộ ra rất nhanh. Nội dung này cũng là một đoạn mẫu quen thuộc trong các bài giới thiệu về substitution cipher.
- Sau khi hoàn tất bảng thế, áp dụng lại cho dòng cuối là ra chuỗi cần nộp. Nếu challenge bọc thêm wrapper thì chỉ việc đặt chuỗi đó vào đúng format.

## Kết quả
Khôi phục được toàn bộ plaintext và suy ra flag theo format `gaslightCTF{...}`.
