### README: DỰ Án Machine Learning - Predicting Problematic Internet Use (PIU)

---

### **Giới thiệu**
Dự án này sử dụng dữ liệu từ Child Mind Institute nhằm dự đoán mức độ sử dụng Internet có vấn đề (Problematic Internet Use - PIU) ở trẻ em. Mục tiêu chính là xây dựng mô hình học máy để dự đoán chỉ số `sii` - chỉ số thể hiện mức độ PIU - dựa trên các thông tin nhân khẩu học, chỉ số thể chất và các đặc trưng liên quan khác.

### **Cách tiếp cận**
Dự án triển khai pipeline từ tiền xử lý dữ liệu, xây dựng đặc trưng (feature engineering), huấn luyện mô hình, tối ưu hóa ngưỡng, và tạo file kết quả submission.

---

### **Các bước triển khai**
#### 1. **Tiền xử lý dữ liệu**
- **Xử lý giá trị thiếu**:
  - `KNNImputer`: Điền giá trị thiếu cho các đặc trưng số (numerical features).
  - `SimpleImputer`: Điền giá trị thiếu cho các đặc trưng phân loại (categorical features).
- **Mã hóa các giá trị phân loại**:
  - Biến đổi các mùa (`Spring`, `Summer`, `Fall`, `Winter`) thành các giá trị số (`1, 2, 3, 4`).
- **Loại bỏ cột**:
  - Bỏ các cột có tỷ lệ giá trị thiếu vượt quá 40%.
  - Loại bỏ các đặc trưng có tương quan cao để giảm đa cộng tuyến (multicollinearity).

#### 2. **Xây dựng đặc trưng (Feature Engineering)**
- Tạo các đặc trưng mới bằng cách kết hợp các đặc trưng hiện có, ví dụ:
  - `BMI_Age`: Tích giữa BMI và tuổi.
  - `Hydration_Status`: Tỷ lệ giữa tổng nước cơ thể (`BIA-BIA_TBW`) và cân nặng (`Physical-Weight`).
  - `Muscle_to_Fat`: Tỷ lệ giữa khối lượng cơ xương (`BIA-BIA_SMM`) và khối lượng mỡ.

#### 3. **Tích hợp dữ liệu chuỗi thời gian**
- **AutoEncoder**: Dùng mô hình AutoEncoder để giảm chiều dữ liệu chuỗi thời gian từ các file `.parquet`.
- Dữ liệu sau khi giảm chiều được gắn thêm vào dữ liệu huấn luyện và kiểm tra (`df_train` và `df_test`).

#### 4. **Huấn luyện mô hình**
- Các mô hình được sử dụng:
  - **XGBoost**
  - **GradientBoostingRegressor**
  - **CatBoostRegressor**
- Áp dụng K-Fold Cross-Validation để đánh giá và tối ưu hóa mô hình.

#### 5. **Tối ưu hóa ngưỡng (Threshold Optimization)**
- Tối ưu ngưỡng dựa trên thước đo **Quadratic Weighted Kappa (QWK)**:
  - Sử dụng hàm `scipy.optimize.minimize` để tìm ngưỡng tối ưu.

#### 6. **Dự đoán và tạo file submission**
- Dự đoán trên tập kiểm tra (`df_test`).
- Biến đổi kết quả dự đoán thành các giá trị phân loại dựa trên ngưỡng tối ưu.
- Xuất kết quả thành file `submission.csv`.

---

### **Kết quả**
- **Điểm QWK tối ưu trên tập huấn luyện**: `0.529`.

---

### **Cấu trúc dự án**
- **notebook.ipynb**: File chứa toàn bộ pipeline xử lý dữ liệu, huấn luyện mô hình và tối ưu.
- **data/**: Thư mục chứa dữ liệu đầu vào, bao gồm:
  - `train.csv`: Dữ liệu huấn luyện.
  - `test.csv`: Dữ liệu kiểm tra.
  - `data_dictionary.csv`: Từ điển dữ liệu.
  - `series_train.parquet`: Dữ liệu chuỗi thời gian tập huấn luyện.
  - `series_test.parquet`: Dữ liệu chuỗi thời gian tập kiểm tra.
- **submission.csv**: File kết quả cuối cùng để nộp trên nền tảng Kaggle.

---

### **Yêu cầu môi trường**
#### 1. **Thư viện Python**
Dự án sử dụng các thư viện sau:
- `numpy`
- `pandas`
- `matplotlib`
- `seaborn`
- `scikit-learn`
- `xgboost`
- `catboost`
- `torch`
- `tqdm`

#### 2. **Cách cài đặt**
```bash
pip install numpy pandas matplotlib seaborn scikit-learn xgboost catboost torch tqdm
```

---

### **Hướng dẫn chạy dự án**
1. **Tải dữ liệu**
   - Đảm bảo các file dữ liệu nằm trong thư mục `data/`.
2. **Chạy pipeline**
   - Chạy notebook hoặc script Python theo thứ tự để tạo file `submission.csv`.
3. **Nộp kết quả**
   - Upload file `submission.csv` lên nền tảng Kaggle để kiểm tra kết quả.

---

### **Liên hệ**
Nếu có bất kỳ câu hỏi hoặc vấn đề nào liên quan đến dự án, vui lòng liên hệ qua email: **[your-email@example.com]**.

