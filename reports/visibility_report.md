# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 15.61 khớp có v > 0 mỗi người
- Tổng: v=2 322 | v=1 115 | v=0 39

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 6 | 0 | 21% |
| 1 | left_eye | 20 | 8 | 0 | 29% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 9 | 19 | 0 | 68% |
| 4 | right_ear | 14 | 14 | 0 | 50% |
| 5 | left_shoulder | 24 | 4 | 0 | 14% |
| 6 | right_shoulder | 26 | 2 | 0 | 7% |
| 7 | left_elbow | 23 | 5 | 0 | 18% |
| 8 | right_elbow | 24 | 4 | 0 | 14% |
| 9 | left_wrist | 19 | 9 | 0 | 32% |
| 10 | right_wrist | 17 | 10 | 1 | 36% |
| 11 | left_hip | 21 | 4 | 3 | 14% |
| 12 | right_hip | 22 | 3 | 3 | 11% |
| 13 | left_knee | 16 | 5 | 7 | 18% |
| 14 | right_knee | 18 | 3 | 7 | 11% |
| 15 | left_ankle | 12 | 7 | 9 | 25% |
| 16 | right_ankle | 15 | 4 | 9 | 14% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
