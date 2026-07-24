# Bài tập lớn: Nhận Diện Chữ Số Viết Tay — MNIST

> Đề tài số 6 | Môn Nhập môn Trí Tuệ Nhân Tạo  
> Xây dựng và so sánh mô hình MLP & CNN để nhận diện chữ số viết tay, tích hợp ứng dụng web vẽ và nhận diện trực tiếp.

---

## Giới Thiệu

Dự án huấn luyện hai kiến trúc mạng nơ-ron trên bộ dữ liệu MNIST (42.000 ảnh, nguồn Kaggle) và triển khai thành ứng dụng web Flask — người dùng vẽ chữ số lên màn hình, AI nhận diện ngay lập tức.

**Hai mô hình được so sánh:**
- **MLP** (Multi-Layer Perceptron) — mạng nơ-ron đa lớp truyền thống  
- **CNN** (Convolutional Neural Network) — mạng tích chập, khai thác cấu trúc không gian của ảnh

---

##  Cấu Trúc Thư Mục

```
mnist/
│
├── train_model.py        # Script huấn luyện mô hình CNN
├── app.py                # Flask web app — nhận diện qua giao diện vẽ
│
├── mnist_cnn.keras       # Mô hình CNN đã huấn luyện  ← tải về (xem phần bên dưới)
├── mnist_mlp.keras       # Mô hình MLP đã huấn luyện  ← tải về (xem phần bên dưới)
│
├── train.csv             # Dữ liệu huấn luyện (Kaggle) ← không push lên GitHub
│
├── templates/
│   └── index.html        # Giao diện web vẽ chữ số
│
├── debug_input.png       # Ảnh debug — AI đang nhìn thấy gì (tự sinh ra khi chạy)
└── requirements.txt      # Danh sách thư viện
```

---

## Kiến Trúc Mô Hình

### MLP — `mnist_mlp.keras`
```
Input (28×28)
    → Flatten (784)
    → Dense 128 — ReLU
    → Dense 64  — ReLU
    → Dense 10  — Softmax
```

### CNN — `mnist_cnn.keras`
```
Input (28×28×1)
    → Conv2D 32 filter (3×3) — ReLU
    → MaxPooling2D (2×2)
    → Conv2D 64 filter (3×3) — ReLU
    → MaxPooling2D (2×2)
    → Flatten
    → Dense 128 — ReLU
    → Dropout (0.5)
    → Dense 10  — Softmax
```

---

##  Hướng Dẫn Cài Đặt và Chạy

### 1. Clone repository

```bash
git clone https://github.com/<tên-của-bạn>/mnist-digit-recognition.git
cd mnist-digit-recognition
```

### 2. Cài đặt thư viện

```bash
pip install -r requirements.txt
```

### 3. Tải file mô hình (nếu không tự huấn luyện)

Tải hai file `mnist_cnn.keras` và `mnist_mlp.keras` từ [Releases](../../releases) rồi đặt vào thư mục gốc của dự án.

### 4. Chạy ứng dụng web

```bash
python app.py
```

Mở trình duyệt và truy cập: **http://localhost:5000**

> Ứng dụng tự động nạp model theo thứ tự ưu tiên:  
> `mnist_cnn.keras` → `mnist_mlp.keras` → `mnist_mlp.h5`

---

##  Tự Huấn Luyện Lại Mô Hình

Nếu muốn tự huấn luyện từ đầu:

**Bước 1:** Tải file `train.csv` từ Kaggle:  
 https://www.kaggle.com/competitions/digit-recognizer/data

**Bước 2:** Đặt file vào thư mục gốc của dự án.

**Bước 3:** Chạy script huấn luyện:

```bash
python train_model.py
```

Script sẽ tự động chia 80/20, huấn luyện 5 epoch và lưu ra `mnist_cnn.keras`.

---

## Kết Quả

| Mô hình | Test Accuracy | Số tham số | Thời gian/epoch |
|---------|:------------:|:----------:|:---------------:|
| MLP     | ~97.5%       | ~118.000   | ~5 giây         |
| CNN     | ~99.0%       | ~421.000   | ~20 giây        |

> CNN cho độ chính xác cao hơn nhờ khai thác cấu trúc không gian của ảnh, nhưng tốn thời gian huấn luyện hơn MLP.

---

##  Cách Hoạt Động Của Ứng Dụng Web

Khi người dùng vẽ xong và bấm **Nhận diện**, ứng dụng xử lý ảnh qua 4 bước:

1. **Nhận ảnh từ Canvas HTML5** — chuyển về thang độ xám (grayscale)  
2. **Xác định vùng bao (bounding box)** — tìm vùng chứa nét vẽ (pixel > 30)  
3. **Căn giữa chuẩn MNIST** — thu phóng giữ tỉ lệ vào ô 20×20, đặt lên nền đen 28×28  
4. **Dự đoán** — chuẩn hóa về [0,1], đưa vào mô hình, trả về chữ số và độ tin cậy

> File `debug_input.png` được tạo tự động sau mỗi lần nhận diện — mở file này lên để xem chính xác AI đang nhìn thấy gì.

---

##  Requirements

```
tensorflow>=2.10
flask
numpy
pandas
pillow
scikit-learn
```

Hoặc cài một lệnh:
```bash
pip install tensorflow flask numpy pandas pillow scikit-learn
```

---

##  Nhóm Thực Hiện

Trịnh Ngọc Nga - 24100065
Bùi Khánh Linh - 24100543

**Giảng viên hướng dẫn:** ThS. Phan Thị Hoài 
**Môn học:** Nhập môn Trí Tuệ Nhân Tạo — Năm học 2025-2026
