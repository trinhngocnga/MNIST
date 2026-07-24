# Bài tập lớn: Nhận Diện Chữ Số Viết Tay — MNIST

> **Đề tài số 6 | Môn Nhập môn Trí Tuệ Nhân Tạo**  
> Xây dựng và so sánh mô hình MLP & CNN để nhận diện chữ số viết tay, tích hợp ứng dụng web vẽ và nhận diện trực tiếp.

## Giới Thiệu
Dự án huấn luyện hai kiến trúc mạng nơ-ron trên bộ dữ liệu chuẩn MNIST (gồm 60.000 ảnh huấn luyện và 10.000 ảnh kiểm thử từ `tensorflow.keras.datasets`) và triển khai thành ứng dụng web Flask — người dùng vẽ chữ số lên màn hình, AI nhận diện ngay lập tức.

**Hai mô hình được triển khai:**
- **MLP** (Multi-Layer Perceptron) — mạng nơ-ron đa lớp cơ bản.
- **CNN** (Convolutional Neural Network) — mạng tích chập, khai thác đặc trưng không gian của ảnh.

---

## 📂 Cấu Trúc Thư Mục
```text
mnist/
│
├── train_model.py        # Script xây dựng, huấn luyện và vẽ biểu đồ đánh giá mô hình
├── app.py                # Máy chủ Flask web app — xử lý ảnh và nhận diện
│
├── mnist_cnn.keras       # Mô hình CNN đã huấn luyện (Độ chính xác > 98%)
├── mnist_mlp.keras       # Mô hình MLP đã huấn luyện (Độ chính xác > 95%)
│
├── templates/
│   └── index.html        # Giao diện web vẽ chữ số (Frontend)
│
├── debug_input.png       # Ảnh debug — AI đang nhìn thấy gì (tự sinh ra khi chạy Web)
└── requirements.txt      # Danh sách thư viện cần thiết

Kiến Trúc Mô Hình
MLP — mnist_mlp.keras
Input (28×28) → Flatten (784)

Hidden 1: Dense 128 — ReLU

Hidden 2: Dense 64  — ReLU

Output: Dense 10  — Softmax

CNN — mnist_cnn.keras
Input (28×28×1)

Conv Block 1: Conv2D 32 filter (3×3) ReLU → MaxPooling2D (2×2)

Conv Block 2: Conv2D 64 filter (3×3) ReLU → MaxPooling2D (2×2)

Flatten

Hidden: Dense 128 — ReLU → Dropout (0.5)

Output: Dense 10  — Softmax

Hướng Dẫn Cài Đặt và Chạy
1. Cài đặt môi trường
Đảm bảo bạn đã cài đặt Python 3.8+. Mở Terminal và cài đặt các thư viện:

Bash
pip install -r requirements.txt
(Hoặc cài trực tiếp: pip install tensorflow flask numpy pillow matplotlib)

2. Chạy ứng dụng web nhận diện
Mở Terminal tại thư mục dự án và chạy:

Bash
python app.py
Mở trình duyệt và truy cập: http://127.0.0.1:5000

Lưu ý: Code app.py được thiết kế thông minh để tự động tìm và nạp model theo thứ tự ưu tiên: mnist_cnn.keras → mnist_mlp.keras → mnist_mlp.h5.

3. Tự huấn luyện lại mô hình (Tùy chọn)
Nếu bạn muốn tự huấn luyện mô hình từ đầu để xuất ra biểu đồ Loss/Accuracy và Confusion Matrix:

Bash
python train_model.py
Script sẽ tự động tải dữ liệu MNIST chuẩn từ Keras (không cần tải thủ công), huấn luyện và lưu đè file mô hình mới.

Cách Hoạt Động Của Ứng Dụng Web (app.py)
Khi người dùng vẽ xong và bấm Dự đoán, ứng dụng xử lý ảnh qua 4 bước chuẩn hóa:

Đọc ảnh từ Canvas: Chuyển đổi dữ liệu Base64 thành ảnh xám (Grayscale).

Cắt ảnh (Bounding box): Thuật toán tìm vùng chứa nét vẽ (các pixel có độ sáng > 30) và cắt bỏ khoảng đen thừa.

Căn giữa chuẩn MNIST: Thu phóng nét vẽ giữ nguyên tỉ lệ (Aspect Ratio) vào khung 20x20, sau đó đặt chính giữa bức nền đen 28x28.

Dự đoán: Chuẩn hóa pixel về khoảng [0,1], định dạng lại kích thước (reshape) tương thích tự động với MLP hoặc CNN, và đưa ra kết quả.

Mẹo Debug: File debug_input.png được tạo tự động trong thư mục dự án sau mỗi lần bấm dự đoán. Hãy mở file này để kiểm tra xem nét chữ của bạn đã được căn giữa chuẩn chưa!

Nhóm Thực Hiện
Trịnh Ngọc Nga - 24100065
Bùi Khánh Linh - 24100543

Giảng viên hướng dẫn: ThS. Phan Thị Hoài

Môn học: Nhập môn Trí Tuệ Nhân Tạo (CSE703041) — Năm học 2025-2026
