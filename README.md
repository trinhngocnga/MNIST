Bài tập lớn: Nhận diện chữ số viết tay MNIST bằng MLP

Đề tài số 6 – Môn Nhập môn Trí tuệ nhân tạo
Trường Công nghệ Thông tin – Đại học Phenikaa

Giới thiệu

Đề tài xây dựng hệ thống nhận diện chữ số viết tay (0–9) sử dụng tập dữ liệu MNIST.

Mô hình chính của đề tài là Multi-Layer Perceptron (MLP). Ngoài ra, hai mô hình Convolutional Neural Network (CNN) và Random Forest (RF) được sử dụng làm mô hình so sánh nhằm đánh giá hiệu quả của MLP.

Hệ thống gồm hai phần:

Huấn luyện và đánh giá các mô hình
Ứng dụng Web Flask cho phép người dùng vẽ chữ số và dự đoán trực tiếp.
Thành viên nhóm

Nhóm 06

Trịnh Ngọc Nga – 24100065
Bùi Khánh Linh – 24100543

Giảng viên hướng dẫn

ThS. Phan Thị Hoài

Cấu trúc thư mục
MNIST_PROJ/
│
├── app.py                 # Flask Web App dự đoán chữ số
├── train_model.py         # Huấn luyện các mô hình và lưu model
│
├── train.csv.zip          # Bộ dữ liệu MNIST (Kaggle)
│
├── mnist_mlp.keras        # Mô hình MLP đã huấn luyện
├── mnist_cnn.keras        # Mô hình CNN đã huấn luyện
├── mnist_rf.joblib        # Mô hình Random Forest đã huấn luyện
│
└── templates/
    └── index.html         # Giao diện Web
Các mô hình sử dụng
1. Multi-Layer Perceptron (MLP)

Là mô hình chính của đề tài.

Kiến trúc:

Input (784)

↓

Dense(256, ReLU)

↓

Dense(128, ReLU)

↓

Dropout(0.2)

↓

Dense(64, ReLU)

↓

Output(10, Softmax)
2. Convolutional Neural Network (CNN)

Được sử dụng làm mô hình so sánh.

Kiến trúc gồm:

Convolution
MaxPooling
Flatten
Dense
Softmax
3. Random Forest

Sử dụng thuật toán Random Forest của Scikit-learn làm mô hình Machine Learning truyền thống để so sánh với Deep Learning.

Thiết lập huấn luyện

Các tham số chính:

Optimizer: Adam
Loss Function: Categorical Crossentropy (MLP, CNN)
Metric: Accuracy
Epoch: (theo train_model.py)
Batch Size: (theo train_model.py)
Hướng dẫn cài đặt

Yêu cầu:

Python 3.9 trở lên

Cài đặt các thư viện:

pip install tensorflow
pip install flask
pip install numpy
pip install pandas
pip install pillow
pip install matplotlib
pip install seaborn
pip install scikit-learn
pip install joblib

Hoặc

pip install tensorflow flask numpy pandas pillow matplotlib seaborn scikit-learn joblib
Hướng dẫn chạy chương trình
Bước 1. Giải nén dữ liệu

Giải nén file

train.csv.zip

thu được

train.csv

đặt cùng thư mục với

train_model.py
Bước 2. Huấn luyện mô hình

Nếu muốn huấn luyện lại từ đầu:

python train_model.py

Sau khi hoàn thành sẽ sinh ra các file:

mnist_mlp.keras

mnist_cnn.keras

mnist_rf.joblib

Đồng thời chương trình sẽ hiển thị các kết quả đánh giá như:

Accuracy
Loss
Confusion Matrix
Classification Report
So sánh các mô hình
Bước 3. Chạy ứng dụng Web

Thực hiện:

python app.py

Nếu thành công sẽ xuất hiện:

Running on http://127.0.0.1:5000

Mở trình duyệt và truy cập:

http://127.0.0.1:5000
Hướng dẫn sử dụng
Mở giao diện Web.
Dùng chuột vẽ một chữ số từ 0–9.
Nhấn nút Predict.
Hệ thống sẽ:
Tiền xử lý ảnh.
Chuẩn hóa về kích thước 28×28.
Đưa ảnh vào mô hình MLP.
Hiển thị kết quả dự đoán.
Quy trình xử lý ảnh

Ảnh vẽ trên Canvas được xử lý theo các bước:

Nhận ảnh từ HTML Canvas dưới dạng Base64.
Chuyển sang ảnh Grayscale.
Đảo màu để phù hợp với MNIST.
Cắt vùng chứa chữ số (Bounding Box).
Resize về 20×20.
Đặt vào khung ảnh 28×28.
Chuẩn hóa giá trị pixel về khoảng [0,1].
Chuyển thành vector đầu vào của mô hình.
Dự đoán bằng mô hình MLP.
Kết quả

Đề tài so sánh ba mô hình:

Multi-Layer Perceptron (MLP)
Convolutional Neural Network (CNN)
Random Forest (RF)

Các tiêu chí đánh giá:

Accuracy
Precision
Recall
F1-score
Confusion Matrix

Qua kết quả thực nghiệm, MLP được lựa chọn là mô hình chính cho ứng dụng Web, trong khi CNN và Random Forest đóng vai trò mô hình đối chứng để đánh giá hiệu quả của phương pháp đề xuất.
