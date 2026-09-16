# Mini guideline - nhóm: ______  |  người gán: Nguyễn Thái Dương  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

* Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
* Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
* Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
* Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
* Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
* Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống                                        | Luật nhóm bạn chọn                                                                                                                                    | Vì sao                                                                                            |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Hông của người mặc quần áo dài                    | Nếu hông còn trong khung ảnh nhưng bị quần áo che, vẫn đặt điểm và chọn `v=1`.                                                                        | Hông vẫn có vị trí giải phẫu có thể ước lượng, không nên xoá chỉ vì quần áo che.                  |
| Tai bị tóc hoặc mũ bảo hiểm che một phần          | Nếu tai còn trong khung nhưng bị che, đặt điểm theo vị trí tai và chọn `v=1`.                                                                         | Tai là một trong các khớp có `%v=1` cao, nên cần thống nhất rõ cách xử lý.                        |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài mép ảnh chọn `v=0`; các khớp còn trong ảnh vẫn phải gán.                                                                           | Không được xem toàn bộ người là `v=0` chỉ vì phần chân bị cắt khỏi ảnh.                           |
| Cổ tay nằm sau tay lái / sau thân mình            | Nếu cổ tay vẫn nằm trong khung ảnh nhưng bị che, vẫn đặt điểm ở vị trí ước lượng và chọn `v=1`.                                                       | Có thể dựa vào hướng của cẳng tay và bàn tay để ước lượng vị trí khớp.                            |
| Hai người chồng lên nhau                          | Xác định từng người trước, sau đó đặt 17 điểm theo đúng cơ thể của từng người; trái/phải tính theo cơ thể người.                                      | Tránh gán nhầm keypoint của người này sang người kia và tránh đảo trái/phải theo vị trí trên ảnh. |
| Người nhỏ đến mức nào thì không gán nữa           | Nếu vẫn xác định được người và vị trí các khớp thì vẫn gán; nếu quá nhỏ/mờ đến mức không thể xác định có căn cứ thì đánh dấu để review, không tự xoá. | Tránh tự đặt một ngưỡng kích thước không có trong guideline chung.                                |

**Ảnh mẫu:** Chèn screenshot CVAT tương ứng vào từng dòng trên trước khi nộp.
*Lưu ý: file kết quả hiện có không chứa screenshot CVAT, nên phần ảnh mẫu cần Nguyễn Thái Dương tự chèn từ các ảnh đã kiểm tra trên CVAT.*

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `1`, khớp `left_ear`

* Mơ hồ ở chỗ nào: `left_ear` có khác trạng thái visibility so với gold.
* Bạn quyết thế nào: Nếu tai vẫn nằm trong khung nhưng bị che thì giữ điểm và dùng `v=1`.
* Vì sao: `v=0` chỉ dùng khi khớp thực sự nằm ngoài ảnh; bị che không đồng nghĩa với ra ngoài ảnh.
* Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể học rằng tai bị che thì phải xoá thay vì học vị trí tai khi bị che.

### Ca 2 - ảnh `train_04`, người thứ `1`, khớp `left_wrist`

* Mơ hồ ở chỗ nào: Vị trí cổ tay bị che và có sai khác vị trí với gold khoảng 38 px.
* Bạn quyết thế nào: Dựa vào hướng của cẳng tay và bàn tay để ước lượng đúng vị trí khớp cổ tay; nếu còn trong ảnh thì dùng `v=1`.
* Vì sao: Điểm phải nằm ở vị trí khớp giải phẫu, không phải ở mép vùng bị che.
* Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể học vị trí cổ tay theo vùng che thay vì vị trí khớp thực tế.

### Ca 3 - ảnh `train_13`, người thứ `1`, khớp `left_ear`

* Mơ hồ ở chỗ nào: `left_ear` trong gold có `v=0`; ảnh này cũng có nhiều keypoint bị gold đánh dấu `v=0` và OKS của người thứ 1 là `0.7704`.
* Bạn quyết thế nào: Kiểm tra từng khớp với mép ảnh; chỉ dùng `v=0` khi khớp thực sự nằm ngoài khung.
* Vì sao: Không nên dùng `v=0` chỉ vì không nhìn thấy khớp; cần phân biệt rõ bị che và nằm ngoài ảnh.
* Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể học sai visibility, bỏ qua những keypoint vẫn có thể ước lượng vị trí.

## 4. Sau khi so visibility report với bạn cùng nhóm

* Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `69%` / họ `___%`)
* Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Cần so bảng visibility của hai người để kết luận. Tuy nhiên kết quả của tôi cho thấy `left_ear` có `%v=1` cao nhất (69%), tiếp theo là `right_ear` 48%, nên đây là vị trí cần thống nhất guideline rõ nhất.
* Luật mới bổ sung vào mục 2 sau khi thống nhất: **Bị che nhưng còn trong khung ảnh thì `v=1` và vẫn đặt chấm; chỉ `v=0` khi khớp thực sự nằm ngoài mép ảnh. Đặc biệt áp dụng thống nhất cho tai, cổ tay và hông.**
