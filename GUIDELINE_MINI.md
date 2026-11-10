# Mini guideline — bài làm cá nhân



## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi và bán tải | ID 0; không xếp bán tải vào truck |
| `truck` | Xe tải chở hàng và xe đầu kéo | ID 1; phân biệt thùng hàng với thân van |
| `bus` | Xe buýt và minibus chở khách | ID 2; ca khó phân biệt minivan cần hỏi giảng viên |
| `van` | Xe van, minivan chở hàng hoặc người | ID 3; không tự suy từ màu xe hoặc kích thước đơn thuần |

## Quy tắc chung

- Box bám sát phần thân xe nhìn thấy, mỗi xe một box, hạn chế nền đường và không gồm bóng đổ.
- Vật thể bị che khuất: gán phần nhìn thấy nếu nhận diện được lớp; không đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: dừng box tại rìa, không suy đoán phần ngoài khung.
- Vật thể rất nhỏ/mơ hồ: không đoán lớp; ghi vị trí và hỏi giảng viên nếu không đủ bằng chứng.
- Xe đỗ vẫn được gán theo loại xe như GUIDE.md; không gán người, xe máy, xe đạp, ảnh xe trên quảng cáo hoặc phản chiếu.
- Chỉ áp dụng vùng bỏ qua nếu giảng viên xác nhận; không tự bỏ xe vì gold thiếu box.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` — dãy xe bên trái | Xe đỗ nhưng gold không có box tương ứng | Đề xuất giữ theo GUIDE; chờ giảng viên xác nhận vùng gán nhãn | Trạng thái đỗ không làm thay đổi lớp hoặc tự loại xe khỏi schema |
| `drive_001` — xe trắng thân cao bên trái, gần giữa ảnh | Phân biệt van với xe khách nhỏ | Chưa chốt; cần xem phóng to và hỏi nếu vẫn mơ hồ, nhãn hiện tại là van | Không đủ căn cứ để đổi thành bus chỉ vì thân xe cao |
| `drive_001` — xe gần cột ở phía phải | Một phần xe bị cột che | Đề xuất giữ box theo phần nhìn thấy nếu nhận diện được car; học viên xác nhận các cạnh | Che khuất không tự loại vật thể; không suy đoán phần bị che |
