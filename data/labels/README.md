# Ground-truth labels

Hai notebook dùng cùng định dạng CSV:

```csv
frame,label
0,OPEN
1,OPEN
2,CLOSED
3,UNKNOWN
```

Trong notebook, chạy `annotate_video(...)` và dùng:

- `O`: mắt mở (`OPEN`)
- `C`: mắt nhắm (`CLOSED`)
- `U`: không xác định (`UNKNOWN`)
- `Space`: lặp nhãn frame trước
- `Q`: lưu và thoát

Không tự gán mọi frame trong video `buon_ngu` thành `CLOSED`. Không chia ngẫu
nhiên các frame liền nhau sang train/test; hãy chia theo video hoặc session.

Các nhãn dự kiến:

- `tinh_tao_eye_state.csv` — train
- `buon_ngu_eye_state.csv` — train
- `tinh_tao1_eye_state.csv` — validation
- `buon_ngu1_eye_state.csv` — validation
- cần quay và gán nhãn thêm test session độc lập
