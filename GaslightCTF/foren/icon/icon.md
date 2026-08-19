# icon

## Tóm tắt
Hint trong EXIF yêu cầu ghép các `Title` lại với nhau. Một title gần như plaintext, title còn lại phải decode thêm trước khi ghép.

## Hướng giải
- Đọc metadata và thấy gợi ý: "decode and put the titles together".
- Title thứ nhất đã lộ sẵn nhiều ký tự của `gaslightCTF{...}`.
- Title thứ hai sau khi decode base64 vẫn chưa ra nghĩa; áp dụng thêm Atbash thì phần còn thiếu hiện ra.
- Chồng hai chuỗi lên nhau là có toàn bộ flag.

## Kết quả
Overlay hai chuỗi metadata cho ra flag theo format `gaslightCTF{...}`.
