# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Thái Dương   Nhóm: ______   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số                       |         Giá trị |
| ---------------------------- | --------------: |
| Số ảnh đã gán                |              20 |
| Số skeleton                  |              29 |
| v=2 / v=1 / v=0              |  334 / 124 / 35 |
| Thời gian trung bình mỗi ảnh | Chưa có dữ liệu |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 69%
2. `right_ear`: 48%
3. `left_eye`: 31%

Các khớp này có tỷ lệ `v=1` cao vì thường bị che, đặc biệt là tai. Tuy nhiên, `%v=1` cao không có nghĩa đây chắc chắn là những khớp khó xác định vị trí giải phẫu nhất. Ví dụ `left_eye` có 31% `v=1`, nhưng khi còn nhìn thấy các phần đầu và khuôn mặt thì vị trí mắt vẫn có thể xác định tương đối rõ.

## 2. Chấm với gold

| Chỉ số                |         Trước rework |           Sau rework |
| --------------------- | -------------------: | -------------------: |
| OKS trung bình        |      Chưa có dữ liệu |               0.9156 |
| OKS@0.50              |      Chưa có dữ liệu |               1.0000 |
| OKS@0.75              |      Chưa có dữ liệu |               1.0000 |
| Lỗi `dao_trai_phai`   | Chưa có số đếm riêng | Chưa có số đếm riêng |
| Lỗi `nham_nguoi`      | Chưa có số đếm riêng | Chưa có số đếm riêng |
| Lỗi `xoa_khop_bi_che` | Chưa có số đếm riêng | Chưa có số đếm riêng |

Sau rework, evaluator ghép được 29/29 người, `missing_people = 0`, `extra_people = 0`. Các finding được ghi trong JSON gồm 66 trường hợp gold không gán khớp, 51 trường hợp cờ visibility khác gold và 20 trường hợp lệch nhẹ.

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

* `train_01` - người thứ 1 - kiểm tra lại các keypoint đầu và tai, đặc biệt các điểm có visibility khác gold.
* `train_04` - người thứ 1 - `left_wrist`: kiểm tra và chỉnh vị trí cổ tay dựa vào hướng cẳng tay/bàn tay thay vì đặt theo vùng bị che.
* `train_13` - người thứ 1 - kiểm tra lại các keypoint đầu/tai và các điểm `v=0` để phân biệt bị che với nằm ngoài khung.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh `train_02`. Đây là ảnh cần kiểm tra lại vì script kiểm nhãn phát hiện dấu hiệu đảo `left/right` ở vai và hông. Nguyên nhân có thể là khi nhìn ảnh đã xác định trái/phải theo phía của bức ảnh thay vì theo cơ thể người; guideline yêu cầu trái/phải phải tính theo cơ thể người.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng:

| Khớp        | Bạn |              Họ |           Lệch | Nguyên nhân (guideline hay gán sai?) |
| ----------- | --: | --------------: | -------------: | ------------------------------------ |
| `left_ear`  | 69% | Chưa có dữ liệu | Chưa tính được | Chưa đủ bảng của bạn cùng nhóm       |
| `right_ear` | 48% | Chưa có dữ liệu | Chưa tính được | Chưa đủ bảng của bạn cùng nhóm       |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

* Nếu khớp còn nằm trong khung ảnh nhưng bị vật khác che thì chọn `v=1` và vẫn đặt chấm ở vị trí ước lượng; chỉ chọn `v=0` khi khớp thực sự nằm ngoài mép ảnh.

## 4. Model

| Chỉ số         | yolo26n-pose gốc | Sau fine-tune |   Chênh |
| -------------- | ---------------: | ------------: | ------: |
| pose_mAP50     |           0.8450 |        0.8450 | +0.0000 |
| pose_mAP50-95  |           0.6853 |        0.6908 | +0.0055 |
| pose_precision |           0.9734 |        0.9792 | +0.0058 |
| pose_recall    |           0.8462 |        0.8462 | +0.0000 |
| box_mAP50-95   |           0.8119 |        0.8041 | -0.0078 |

Các số trên được chép từ `eval_model.json`; `chênh = sau fine-tune - gốc`.

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng `0.0055`, từ `0.6853` lên `0.6908`. `pose_mAP50` và pose recall không thay đổi, còn pose precision tăng `0.0058`. Điều này cho thấy sau fine-tune, thay đổi quan sát được chủ yếu nằm ở pose mAP50-95 và precision trên tập test.

2. Ở ngưỡng mAP50-95, `box_mAP50-95 = 0.8041` và `pose_mAP50-95 = 0.6908`, chênh `0.1133`. Vì vậy trong kết quả này, việc xác định người/bbox có điểm cao hơn việc xác định chính xác 17 keypoint. Ở mAP50, `box_mAP50 = 0.9600` còn `pose_mAP50 = 0.8450`, chênh `0.1150`.

3. Một ảnh cần xem lại là `train_13`. OKS giữa model và nhãn của tôi thấp nhất ở ảnh này là `0.613`. Tuy nhiên chỉ từ OKS chưa đủ để kết luận đó là lệch nhẹ, đảo trái/phải, nhầm người hay trượt hẳn; cần đối chiếu trực tiếp ảnh prediction để xác định loại lỗi.

4. Ảnh có OKS thấp nhất giữa nhãn của tôi và model là `train_13`, với `0.613`. Khi đối chiếu gold, `train_13` cũng có một match với OKS `0.7704` và nhiều keypoint bị gold đánh dấu `v=0`. Vì vậy chưa thể nói model hay nhãn của tôi đúng chỉ dựa trên con số; bằng chứng cần dùng là ảnh trực quan và kết quả so với gold.

5. Có. `train_13` vừa là ảnh có OKS nhãn-vs-gold thấp nhất trong các match được ghi nhận (`0.7704`), vừa là ảnh có OKS model-vs-nhãn thấp nhất (`0.613`). Điều này cho thấy `train_13` là một trường hợp khó, có nhiều điểm visibility khác biệt và cần kiểm tra keypoint trực tiếp thay vì chỉ nhìn điểm OKS.

## 5. Một rule evidence bạn đã dùng

Ở `train_04`, người thứ 1, `left_wrist`, cổ tay bị che nên khó xác định chính xác vị trí. Tôi dựa vào phần cẳng tay và bàn tay còn nhìn thấy để ước lượng vị trí khớp cổ tay, thay vì đặt chấm vào mép vùng bị che. Vì cổ tay vẫn nằm trong khung ảnh nên trạng thái phù hợp là `v=1`, không phải `v=0`. Đây cũng là trường hợp evaluator ghi nhận lệch nhẹ so với gold, khoảng 38 px, nên cần ưu tiên kiểm tra vị trí giải phẫu của khớp.
