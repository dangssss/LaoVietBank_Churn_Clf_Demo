# LaoVietBank - Demo Dự Đoán Khách Hàng Rời Bỏ

Repository này chứa demo xây dựng mô hình **Customer Churn Prediction** cho LaoVietBank. Nội dung chính nằm trong notebook và bản PDF export từ notebook, bao gồm cả code, output, biểu đồ và phần giải trình nghiệp vụ/kỹ thuật.

## Nội dung repository

| File | Mô tả |
| --- | --- |
| `Demo.ipynb` | Notebook chứa toàn bộ source code pipeline machine learning. |
| `Laovietbank.pdf` | Bản in/export của notebook, có cả code, output, biểu đồ và phần giải thích trong các ô text. |

## Mục tiêu dự án

Demo hướng tới bài toán dự đoán nhóm khách hàng có khả năng rời bỏ ngân hàng, từ đó hỗ trợ các quyết định giữ chân khách hàng.

- **Giảm tỷ lệ khách hàng rời bỏ:** chủ động phát hiện sớm khách hàng có nguy cơ churn để có chính sách chăm sóc kịp thời.
- **Tối ưu chi phí giữ chân:** tập trung nguồn lực vào nhóm khách hàng rủi ro cao thay vì xử lý dàn trải.
- **Nâng cao trải nghiệm khách hàng:** hiểu các yếu tố ảnh hưởng đến churn để cải thiện sản phẩm và dịch vụ.
- **Tăng lợi thế cạnh tranh:** ứng dụng AI/ML để phản ứng nhanh hơn trước biến động hành vi khách hàng.

## Dữ liệu đầu vào

Notebook sử dụng dữ liệu khách hàng ngân hàng với các nhóm thông tin chính:

- **Thông tin khách hàng:** `CustomerId`, `Gender`, `Age`, `Tenure`.
- **Thông tin tài chính:** `CreditScore`, `Balance`, `EstimatedSalary`, `NumOfProducts`.
- **Hành vi sử dụng dịch vụ:** `IsActiveMember`, `LoginFrequency`, `ExternalTransfers`.
- **Nhãn mục tiêu:** `Exited`, thể hiện khách hàng có rời bỏ hay không.

> Dataset gốc `BankingDataset.xlsx` không nằm trong repository này. Nếu chạy lại notebook, cần đặt dataset đúng đường dẫn hoặc chỉnh lại đường dẫn đọc file trong notebook.

## Luồng xử lý chính

### 1. Thiết lập dự án

Notebook cài và import các thư viện cần thiết như `pandas`, `numpy`, `scikit-learn`, `xgboost`, `lightgbm`, `shap` và `mapie`.

### 2. Tiền xử lý và feature engineering

Các bước xử lý chính:

- Mapping biến phân loại `Gender` sang dạng số.
- Tạo thêm các biến hành vi/tài chính để mô hình có thêm góc nhìn:
  - `BalancePerProduct`: số dư trung bình theo số lượng sản phẩm.
  - `ActivityPerTenure`: tần suất đăng nhập theo thời gian gắn bó.
  - `SalaryPerAge`: thu nhập ước tính theo độ tuổi.
  - `TransferLoginRatio`: tỷ lệ giao dịch chuyển khoản so với tần suất đăng nhập.
- Xử lý outlier bằng IQR clipping cho các biến số quan trọng.

Ý tưởng của phần feature engineering là giúp mô hình nhận diện các tín hiệu như: khách có số dư nhưng ít dùng sản phẩm, khách gắn bó lâu nhưng ít tương tác, hoặc khách đăng nhập nhiều nhưng ít giao dịch tài chính thực sự.

### 3. Phân tích dữ liệu

Notebook thực hiện các bước EDA cơ bản:

- Kiểm tra shape, kiểu dữ liệu và missing values.
- Thống kê mô tả các biến.
- Vẽ phân phối biến số sau khi xử lý outlier.
- Phân tích ma trận tương quan.
- Tính Mutual Information để đánh giá mức liên hệ giữa feature và biến mục tiêu `Exited`.

### 4. Huấn luyện mô hình churn prediction

Notebook chia dữ liệu thành ba tập:

- **Train:** 70%
- **Calibration:** 10%
- **Test:** 20%

Ba mô hình classification được huấn luyện và so sánh:

- Random Forest
- XGBoost
- LightGBM

Vì bài toán churn thường mất cân bằng lớp, notebook dùng các tham số như `class_weight='balanced'` và `scale_pos_weight` để mô hình chú ý hơn đến nhóm khách hàng churn.

### 5. Đánh giá và chọn mô hình

Các mô hình được đánh giá bằng:

- Classification report
- Confusion matrix
- ROC-AUC
- PR-AUC
- Precision, Recall, F1-score cho nhóm churn

Notebook chọn mô hình tốt nhất dựa trên **ROC-AUC**, phù hợp với bài toán classification có dữ liệu mất cân bằng.

### 6. Giải thích mô hình bằng SHAP

SHAP được dùng để trả lời câu hỏi: **vì sao mô hình dự đoán một khách hàng có khả năng rời bỏ?**

Các biểu đồ chính:

- **SHAP bar plot:** xem mức độ quan trọng tổng thể của từng biến.
- **SHAP summary plot:** quan sát chiều tác động của feature đến xác suất churn.
- **SHAP waterfall plot:** giải thích dự đoán cho từng khách hàng cụ thể.

Phần này giúp kết quả mô hình minh bạch hơn, dễ giải thích với bộ phận kinh doanh/chăm sóc khách hàng.

### 7. Tạo khoảng/tập dự đoán tin cậy bằng MAPIE

MAPIE được dùng để bổ sung lớp đánh giá độ chắc chắn cho dự đoán thông qua conformal prediction.

Notebook thực hiện:

- Calibrate mô hình trên calibration set.
- Tạo prediction set với mức tin cậy cấu hình.
- Kiểm tra coverage thực tế so với mức tin cậy kỳ vọng.
- Phân tích độ rộng prediction set theo các mức `alpha`.

Mục tiêu là giúp hệ thống không chỉ đưa ra nhãn churn/không churn, mà còn thể hiện mức độ chắc chắn của dự đoán.

## Cách chạy notebook

1. Mở `Demo.ipynb` bằng Jupyter Notebook, JupyterLab hoặc Google Colab.
2. Cài thư viện theo cell đầu notebook:

```python
!pip install shap mapie xgboost lightgbm -q
```

3. Đảm bảo file dữ liệu `BankingDataset.xlsx` tồn tại đúng đường dẫn notebook đang đọc.
4. Chạy lần lượt các cell từ trên xuống dưới.
5. Nếu chỉ cần xem kết quả đã chạy, mở `Laovietbank.pdf`.

## Artifact khi chạy lại

Khi notebook được chạy lại, thư mục `models/` có thể được sinh ra để lưu:

- Model Random Forest, XGBoost, LightGBM đã train.
- Danh sách feature sử dụng.
- Tên mô hình tốt nhất.
- Mapping cho biến `Gender`.

Các artifact này có thể tái tạo từ notebook nên không bắt buộc phải có sẵn trong repository.
