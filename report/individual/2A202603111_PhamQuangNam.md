# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202603111
- Ngày / CVAT local: 11/11/2026, CVAT local v2.74.1 (chạy Docker ra localhost:8080)
- Công cụ đã dùng: CVAT, Python

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

Ở `medium_instance`, người đi xe máy trong ảnh `000000181542.jpg` (class `person`, bbox export `[93, 163, 28, 96]`) được chọn làm ví dụ. Mask được vẽ bám theo đường bao phần người nhìn thấy và tách riêng khỏi xe máy bên cạnh. Mask được tự vẽ trước, sau đó mới xem gợi ý tự động để kiểm tra ranh giới.

## 3. Một lỗi được tìm thấy và sửa

Trong quá trình tự chấm Medium, một số mask được rà soát thêm để bám sát ranh giới vật thể, đồng thời chú ý đến các vật nhỏ hoặc bị che khuất. Sau đó, bài được lưu và export lại.

Kết quả tự đánh giá mới nhất từ script: Easy mIoU `0.815` (`18.4/20`); Medium metric `0.631` (`16.4/32`); Hard PQ `0.337` (`9.1/30`). Tổng ba tier `43.9/82`. Đây là điểm phản hồi theo reference, không phải điểm chính thức trên 100. Checkpoints chưa có reference trong gói được cấp nên chưa có điểm tự chấm. Không tự ghi PASS/top 3/bonus; không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `cp1_holes`, `000000144300.jpg`: nan hoa/khe trong mask motorcycle | Khe nhìn xuyên thấy nền hay phần chi tiết nằm trong mask vật | Quy tắc trạm: giữ kính/khe trong mask, không khoét tùy tiện | Giữ các chi tiết bên trong mask motorcycle; chỉ tách nền nếu đó là lỗ thật theo ảnh |
| `cp2_slice`, `000000017627.jpg`: các xe cùng class ở sát nhau | Một vùng xe bị nối liền hay hai xe riêng | Cùng class không có nghĩa là cùng instance; đối chiếu đường biên từng xe | Tách thành instance riêng nếu ảnh cho thấy hai xe khác nhau |
| `cp5_occlusion`, `000000336232.jpg`: xe bị che khuất | Các phần nhìn thấy thuộc cùng xe hay hai xe | Một vật bị che vẫn là một instance; chỉ mask phần nhìn thấy | Giữ một instance cho cùng xe, không vẽ phần bị che |