# Bài tập lớn: Nhận Diện Chữ Số Viết Tay MNIST bằng Mạng Nơ-ron Đa lớp (MLP)

> **Đề tài số 6 | Môn Nhập môn Trí Tuệ Nhân Tạo**  
> **Đại học Phenikaa — Trường Công nghệ Thông tin Phenikaa**
---
## Giới Thiệu
Dự án ứng dụng kiến trúc **Mạng Nơ-ron Đa lớp (Multi-Layer Perceptron - MLP)** để giải quyết bài toán phân loại chữ số viết tay (từ 0 đến 9) dựa trên tập dữ liệu chuẩn MNIST (60.000 ảnh huấn luyện và 10.000 ảnh kiểm thử).

Sản phẩm hoàn chỉnh bao gồm:
1. **Chương trình huấn luyện & Đánh giá:** Xây dựng, so sánh các cấu hình kiến trúc MLP khác nhau, xuất đồ thị Loss/Accuracy, Ma trận nhầm lẫn (Confusion Matrix) và phân tích ảnh phân loại sai.
2. **Ứng dụng Web trực quan (Flask App):** Cho phép người dùng tự vẽ chữ số bằng chuột trên giao diện Canvas, hệ thống tự động tiền xử lý nét vẽ và đưa qua mô hình MLP để nhận diện theo thời gian thực.
---
## Nhóm Thực Hiện (Nhóm 06)
* **Trịnh Ngọc Nga** - MSV: 24100065
* **Bùi Khánh Linh** - MSV: 24100543

**Giảng viên hướng dẫn:** ThS. Phan Thị Hoài  
**Lớp tín chỉ:** CSE703041-2-3-25 (N02)

---

## Cấu Trúc Thư Mục
```text
mnist/
│
├── train_model.py        # Script tải dữ liệu, huấn luyện/so sánh các kiến trúc MLP & vẽ biểu đồ
├── app.py                # Máy chủ Web Flask — xử lý ảnh vẽ từ Canvas và nhận diện
│
├── mnist_mlp.keras       # Mô hình MLP tối ưu đã được huấn luyện sẵn (Test Accuracy > 95%)
│
├── templates/
│   └── index.html        # Giao diện Web vẽ chữ số (HTML5 Canvas + Chart.js)
│
├── debug_input.png       # Ảnh debug 28x28 hiển thị nét vẽ thực tế sau khi AI căn giữa
└── requirements.txt      # Danh sách các thư viện Python cần thiết

Kiến Trúc Các Mô Hình MLP
Để tìm ra cấu hình tối ưu theo yêu cầu mở rộng của đề tài, nhóm đã thử nghiệm và so sánh 3 kiến trúc MLP với số lượng lớp ẩn và số nơ-ron khác nhau:
Mô hình 1 (Shallow Network): Input (784) → Dense 128 (ReLU) → Output 10 (Softmax)
Mô hình 2 (Deep Network): Input (784) → Dense 256 (ReLU) → Dense 128 (ReLU) → Output 10 (Softmax)
Mô hình 3 (Deep + Regularization): Input (784) → Dense 256 (ReLU) → Dense 128 (ReLU) → Dense 64 (ReLU) → Dropout (0.2) → Output 10 (Softmax)

Thiết lập huấn luyện chung:
Hàm mất mát (Loss function): Categorical Cross-Entropy
Thuật toán tối ưu (Optimizer): Adam
Đánh giá (Metric): Accuracy

Hướng Dẫn Cài Đặt và Sử Dụng
1. Cài đặt môi trường
Yêu cầu máy tính cài sẵn Python 3.8 trở lên. Mở Terminal / Command Prompt tại thư mục dự án và chạy:
``Bash
pip install -r requirements.txt
(Nếu chưa có file requirements.txt, chạy lệnh: pip install tensorflow flask numpy pillow matplotlib scikit-learn)

2. Khởi động Giao diện Web nhận diện
Mở Terminal và chạy lệnh:
``Bash
python app.py
Trình duyệt sẽ hiển thị thông báo server khởi chạy tại: http://127.0.0.1:5000
Truy cập đường dẫn trên, dùng chuột vẽ một số (0-9) lên bảng đen và chọn Dự đoán.

3. Huấn luyện lại mô hình & Xuất biểu đồ báo cáo (Tùy chọn)
Nếu muốn chạy lại quá trình huấn luyện 3 kiến trúc MLP từ đầu để lấy số liệu cho bài báo cáo:
``Bash
python train_model.py
Script sẽ tự động tải tập dữ liệu MNIST từ Keras, huấn luyện mô hình, lưu file mnist_mlp.keras, đồng thời hiển thị/lưu các biểu đồ Loss/Accuracy và Confusion Matrix.

Quy Trình Tiền Xử Lý Ảnh Trong app.py
Để khắc phục hiện tượng lệch phân phối dữ liệu khi vẽ bằng chuột, ứng dụng áp dụng thuật toán 4 bước chuẩn hóa:
1.Đọc ảnh: Chuyển đổi dữ liệu chuỗi Base64 từ Canvas web thành ảnh xám (Grayscale).
2.Cắt vùng chứa nét vẽ (Bounding Box): Tìm tọa độ các pixel có độ sáng > 30 để cắt bỏ hoàn toàn vùng viền đen thừa.
3.Căn giữa chuẩn MNIST: Thu phóng nét vẽ giữ nguyên tỉ lệ (Aspect Ratio) vào khung 20x20, sau đó đặt chính giữa bức nền đen 28x28.
4.Dự đoán: Chuẩn hóa giá trị pixel về khoảng [0, 1], duỗi thành vector 784 chiều và đưa qua mô hình MLP.
