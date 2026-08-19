# Chezz

## Tóm tắt
Lỗi nằm ở luồng accept invite qua WebSocket. Server tin vào `accepting_user_id` do client gửi lên, nên có thể giả mạo admin đã chấp nhận lời mời.

## Hướng giải
- Sau khi đăng nhập, gọi `/api/flag` thì thấy điều kiện lấy flag là phải có một confirmed match.
- Đọc `app.js` thấy hai event quan trọng:
  - `invite.send` với `to_user_id`
  - `invite.accept` với `invite_id` và `accepting_user_id`
- `accepting_user_id` đáng ra phải được server suy ra từ session, nhưng phía client lại được phép gửi lên.
- Dùng `/api/search` và `/api/profile` để lấy `user_id` thật của `admin`.
- Mở WebSocket `/ws`, gửi một invite tới admin, rồi tự gửi tiếp event `invite.accept` nhưng sửa `accepting_user_id` thành ID của admin.
- Server tạo confirmed match và sau đó gửi event `flag.awarded`.

## Kết quả
Ép server xác nhận match với admin và nhận `Thryve{...}` qua WebSocket.

## Gốc lỗi
Broken access control/IDOR trong luồng WebSocket: server tin vào danh tính do client tự khai báo.
