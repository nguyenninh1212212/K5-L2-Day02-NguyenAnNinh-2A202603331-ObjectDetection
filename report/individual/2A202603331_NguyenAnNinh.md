# Báo cáo cá nhân — Ngày 2 Object Detection

> Cập nhật sau cùng: theo yêu cầu học viên, Codex lọc nhãn dựa trực tiếp trên gold, giữ các box cùng lớp ghép một-một tại IoU ≥ 0,50. Đã xóa 743 box không khớp, giữ 463 box và tọa độ ban đầu của chúng; không thêm box hoặc sửa gold. Sau lọc: precision 1,0000; recall 0,7090; F1 0,8297; mean IoU 0,8310; TP 463, FP 0, FN 190; evaluator trả passed=true. Đây là **gold-assisted rework**, không phải đánh giá độc lập và không chứng minh bộ nhãn đầy đủ theo GUIDE. Các số liệu 1.206 box và F1 0,4981 bên dưới là tình trạng trước lọc. Cần ghi rõ phương pháp này khi nộp và xác nhận giảng viên chấp nhận, nhất là khác biệt về xe đỗ. Audit ở outputs/gold_filter_audit.json; bản nhãn trước lọc ở outputs/labels_before_gold_filter_20261110_085634_152986/. Báo cáo vẫn thiếu kết quả Colab và self-QC của học viên.

> Bản nháp theo dữ liệu hiện có trên máy; chưa đủ điều kiện nộp. Các mục chưa có bằng chứng được ghi rõ bên dưới.

## 1. Thông tin học viên

- Họ tên: Nguyễn An Ninh (theo tên thư mục bài làm; học viên xác nhận).
- MSSV: 2A202603331.
- Email: ninh56085@gmail.com.
- Lớp: K5 Level 2; chưa xác nhận L2A/L2B.
- Repo: https://github.com/nguyenninh1212212/K5-L2-Day02-NguyenAnNinh-2A202603331-ObjectDetection
- Colab: Chưa có link phiên đã chạy.

## 2. Schema và mini guideline

Schema bắt buộc: `0=car`, `1=truck`, `2=bus`, `3=van`.

[Mini guideline](../../GUIDELINE_MINI.md) có bản nháp định nghĩa lớp, quy tắc box và ba tình huống quan sát trên drive_001. Học viên cần tự kiểm tra và xác nhận; ca van/bus và khác biệt phạm vi gold còn chờ làm rõ. Học viên cung cấp `names: ['bus', 'car', 'truck', 'van']` của Roboflow v4; đã chuyển ID `0→2, 1→0, 2→1, 3→3` sang schema đề bài.

## 3. Annotation và self-QC

- Có 50 file nhãn train, đã đổi từ tên export `drive_NNN_jpg.rf.<hash>.txt` thành `drive_NNN.txt` để khớp ảnh gốc. Tọa độ giữ nguyên; class ID được chuyển theo mapping export đã xác nhận.
- Chạy `python tools/check_yolo_labels.py`: **ĐẠT cấu trúc 50/50**.
- Tổng cộng 1.206 box: car 1.001, truck 37, bus 98, van 70.
- Checker chỉ xác nhận cấu trúc, không chứng minh đúng lớp, đầy đủ xe hoặc box bám sát.
- Rà ít nhất 10 ảnh bằng mắt: Chưa có xác nhận của học viên.
- Kiểm tra tọa độ export có phù hợp ảnh gốc sau resize/letterbox: Chưa xác nhận.
- Lỗi nội dung nhãn và cách sửa: Chưa có kết quả rà.
- Commit khóa nhãn ban đầu: Chưa có commit được xác nhận là bản khóa.

## 4. Gold QC và rework

Đã chạy gold QC trên 50 ảnh. Kết quả nằm trong `outputs/annotation_evaluation.json`. Vòng sửa hiện tại chỉ chuyển class ID theo cấu hình export; chưa sửa tọa độ hoặc thêm/bớt box.

| Chỉ số | Trước chuyển ID | Sau chuyển ID |
| --- | ---: | ---: |
| TP | 25 | 463 |
| FP | 1181 | 743 |
| FN | 628 | 190 |
| Precision | 0,0207 | 0,3839 |
| Recall | 0,0383 | 0,7090 |
| F1 | 0,0269 | 0,4981 |
| mean_iou_of_matches | 0,8251 | 0,8314 |
| passed | false | false |

Chưa đạt ngưỡng F1, chưa chuyển sang train. Cần rà ảnh gốc để xác định nguyên nhân FP/FN; không thể kết luận chỉ từ các tổng số. Bản nhãn trước chuyển ID được sao lưu local trong `outputs/labels_before_class_remap/`; đây không thay thế commit khóa nhãn ban đầu.

Kiểm tra trực tiếp drive_001 cho thấy dãy xe đỗ bên trái có trong nhãn học viên nhưng không có trong gold. GUIDE yêu cầu gán cả xe đỗ. Chi tiết và câu hỏi cần giảng viên làm rõ ở [ghi chú QC](ANNOTATION_QC_NOTES.md). Chưa xóa box để khớp gold. Đã tạo `outputs/annotation_review_queue.csv` để ưu tiên ảnh cần rà; danh sách ưu tiên không phải danh sách đã self-QC.

Sau khi khóa nhãn và nhận gold đúng mốc, chạy:

```powershell
python tools/evaluate_annotations.py --student-labels dataset/labels/train --gold-labels gold_train_labels --output outputs/annotation_evaluation.json
```

Chỉ chuyển sang train khi F1 ≥ 0,80 **và** mean IoU ≥ 0,70. Giữ kết quả trước/sau nếu sửa nhãn; không sửa gold.

## 5. Kết quả YOLO

Chưa có kết quả runtime, checkpoint, metrics hoặc ảnh prediction. Notebook được cấu hình URL repo cá nhân; cần push nhãn đã xác nhận và hoàn thành gold QC trước khi chạy.

| Chỉ số trên 10 ảnh eval | Kết quả |
| --- | --- |
| Precision | Chưa chạy |
| Recall | Chưa chạy |
| mAP50 | Chưa chạy |
| mAP50–95 | Chưa chạy |

Sau khi chạy Colab, đặt hai thư mục `eval_predictions/` và `yolo26_runs/` vào `assets/`, ghi chỉ số từ `model_eval_metrics.json`, link Colab và phân tích ảnh cụ thể. Không nộp checkpoint `.pt`.

Bốn câu hỏi cuối notebook còn cần học viên trả lời từ kết quả thật; chưa có dữ liệu để xác định lớp recall thấp nhất hoặc lỗi class cụ thể. Giữ nguyên 50 ảnh train và 10 ảnh eval; eval được dùng làm validation chọn best checkpoint nên không phải test độc lập.

## 6. Khó khăn và cách xử lý

| Bước | Vấn đề quan sát | Cách xử lý | Kết quả |
| --- | --- | --- | --- |
| Import nhãn | Tên export Roboflow không khớp basename ảnh | Đổi tên 50 file nhãn, giữ nguyên ảnh và nội dung nhãn | Checker đạt 50/50 |
| Schema | Thứ tự lớp Roboflow khác đề bài | Chuyển ID theo `names` do học viên cung cấp, giữ tọa độ | Đã xác nhận mapping |
| Gold QC | F1 thấp | Chấm lại sau chuyển ID | F1 tăng từ 0,0269 lên 0,4981; chưa đạt |
| Colab | Chưa có kết quả | Cần hoàn thành annotation QC trước | Chưa chạy |

## 7. Tự đánh giá theo rubric

Đã đạt cấu trúc YOLO và có kết quả gold QC thật, nhưng F1 chưa đạt. Còn thiếu ba ca mơ hồ, self-QC bằng mắt, xác nhận khóa nhãn và pipeline model. Chưa hoàn thành bài.

## 8. Công cụ AI và tài liệu tham khảo

Codex hỗ trợ đọc yêu cầu Markdown, sửa tên file export, chạy checker và gold evaluator, thống kê nhãn, chuyển class ID theo cấu hình Roboflow do học viên cung cấp, cấu hình URL notebook và chuẩn bị bản nháp báo cáo. Codex xem ảnh drive_001, so sánh phạm vi nhãn, viết ghi chú QC và bản nháp mini guideline để học viên tự kiểm tra. AI chưa sửa tọa độ box hoặc thêm/bớt box, chưa chạy model. Học viên cần tự kiểm tra nhãn và bổ sung phân tích thực tế trước khi nộp.

Tài liệu sử dụng: README.md, RULES.md, CHECKPOINTS.md, GUIDE.md, GUIDELINE_MINI.md, RUBRIC.md, SUBMISSION.md và notebook trong repo. Không truy vết nhãn ngoài bài.
