# Hệ thống phát hiện buồn ngủ qua camera

Project gồm hai luồng nghiên cứu chính song song:

1. **White-box:** Haar Eye → Gaussian → Canny → Hough → openness score → Raw/EMA/Kalman.
2. **Black-box:** Haar Face → MediaPipe → chuỗi EAR → Threshold/KNN/SVM/Random Forest.

## Cấu trúc

```text
project/
├── notebooks/
│   ├── 01_whitebox_research.ipynb
│   └── 02_blackbox_research.ipynb
├── data/
│   ├── raw/images/
│   ├── raw/videos/
│   ├── processed/
│   └── labels/
├── models/cascades/
├── assets/audio/
└── docs/
    ├── reports/
    └── references/
```

Hai notebook tự chứa code, không phụ thuộc các file `.py` bên ngoài.

## Cài đặt

```bash
pip install -r requirements.txt
```

Khuyến nghị mở Jupyter từ thư mục project:

```bash
jupyter notebook
```

Sau đó chạy từng notebook từ trên xuống. Các cell demo camera/video được để ở
trạng thái comment để `Run All` không tự mở cửa sổ.

## Thứ tự nghiên cứu

### White-box

1. Khảo sát padding, Gaussian, Canny và Hough.
2. Gán nhãn `OPEN/CLOSED/UNKNOWN` theo frame.
3. So sánh Raw, EMA và Kalman trên cùng tín hiệu độ mở mắt.
4. Chọn `Q`, `R` và ngưỡng bằng validation session.
5. Báo cáo F1 CLOSED, false alarm, flickering và transition lag trên test mới.

### Black-box

1. Gán nhãn từng session và trích EAR bằng MediaPipe.
2. Tạo cửa sổ 13 frame quá khứ.
3. So sánh threshold, KNN, SVM và Random Forest.
4. Tối ưu Random Forest trên validation session.
5. Giải thích bằng feature importance và phiếu bầu cây.
6. Đánh giá một lần trên test session chưa dùng để chỉnh model.

## Lưu ý dữ liệu

Không chia ngẫu nhiên các frame liền nhau vào train/test. Cần chia theo video,
session hoặc người để tránh data leakage. Nếu chỉ dùng video của một người, kết
luận phải giới hạn là mô hình cá nhân hóa, không được tuyên bố tổng quát cho mọi
tài xế.
