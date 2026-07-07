# 📉 Phân tích và Dự báo Khách hàng Rời bỏ (Telco Customer Churn)

## 📌 Giới thiệu dự án
Dự án này tập trung vào việc phân tích tập dữ liệu khách hàng của một công ty viễn thông (Telco) tại bang California nhằm hiểu rõ các nguyên nhân và đặc điểm của những khách hàng đã ngừng sử dụng dịch vụ (Churn). Dựa trên các insight thu được, dự án ứng dụng Machine Learning để xây dựng hệ thống cảnh báo sớm, giúp doanh nghiệp chủ động nhận diện khách hàng có nguy cơ rời bỏ cao và tối ưu hóa ngân sách cho các chiến dịch giữ chân (Retention).

## 🚀 Quy trình thực hiện (Notebook Workflow)
Notebook được thiết kế theo luồng xử lý chuẩn của một dự án Khoa học Dữ liệu, bao gồm 5 phần chính:

### 1. Tải dữ liệu và Kiểm tra tổng quan
* Kết nối và đọc tập dữ liệu gốc từ file `Telco_customer_churn.xlsx`.
* Khám phá cấu trúc dữ liệu: 7.043 khách hàng tại bang California (Mỹ) với hơn 30 trường thông tin chi tiết về nhân khẩu học, dịch vụ và tài chính.

### 2. Tiền xử lý dữ liệu (Data Preprocessing)
* **Ép kiểu dữ liệu:** Chuyển đổi cột `Total Charges` về định dạng số (Numerical).
* **Xử lý giá trị thiếu (Missing Values):**
  * Loại bỏ cột `Churn Reason` do có tới ~73% dữ liệu bị thiếu (tương ứng với nhóm khách hàng hiện vẫn đang sử dụng dịch vụ).
  * Điền logic các giá trị thiếu trong cột `Total Charges` bằng công thức: `Monthly Charges * Tenure Months` (áp dụng cho khách hàng mới ở tháng đầu tiên).

### 3. Phân tích Khám phá Dữ liệu (EDA)
Phân tích trực quan hóa dữ liệu bằng `Plotly` và `Seaborn` để rút ra các insight kinh doanh cốt lõi:
* **Tỉ lệ rời bỏ tổng thể:** Đứng ở mức 26.54%.
* **Phân bố địa lý:** Thành phố Los Angeles tập trung lượng khách hàng lớn nhất, các khu vực có số lượng khách hàng nhỏ lại có tỷ lệ churn cao bất thường.
* **Thời gian gắn bó (Tenure):** Rủi ro cao nhất nằm ở giai đoạn đầu – khoảng một nửa số khách hàng rời đi chỉ trong vòng 10 tháng đầu tiên.
* **Loại hợp đồng & Thanh toán:** Tổ hợp rủi ro cao nhất thuộc về nhóm khách hàng dùng hợp đồng trả từng tháng (Month-to-month, chiếm 88.7% lượng khách rời bỏ) kết hợp với phương thức thanh toán "Electronic check".

### 4. Xây dựng Mô hình Dự báo (Machine Learning)
* Chuẩn bị dữ liệu (Encoding) và xử lý triệt để bài toán mất cân bằng lớp (Imbalanced data với tỷ lệ 73:27) bằng kỹ thuật sinh mẫu nhân tạo **SMOTE**.
* Huấn luyện và đánh giá hai mô hình phân loại mạnh mẽ:
  * **XGBoost Classifier:** Độ chính xác (Accuracy) ~81.33%, Precision ~72%, Recall ~51%.
  * **Random Forest Classifier:** Độ chính xác (Accuracy) ~80.84%, Precision ~64%, Recall ~65%.
* **Lựa chọn mô hình:** Chọn **XGBoost** làm mô hình cuối cùng vì chỉ số Precision cao (72%), giúp doanh nghiệp giảm thiểu tối đa các cảnh báo giả (False Positives), từ đó tránh lãng phí ngân sách chăm sóc khách hàng không cần thiết.

### 5. Dự báo và Lập danh sách ưu tiên
* Ứng dụng mô hình XGBoost để tính toán xác suất rời bỏ (`Churn_Probability`) cho toàn bộ tập dữ liệu hiện tại.
* Sàng lọc nhóm khách hàng **đang hoạt động** nhưng có nguy cơ rời bỏ cao (Xác suất > 0.5) và gắn nhãn `High Risk`.
* Trích xuất Top 10 khách hàng cần được đội ngũ CSKH ưu tiên can thiệp khẩn cấp.

## 🛠️ Công cụ và Thư viện sử dụng
* **Ngôn ngữ:** Python (Google Colab / Jupyter Notebook)
* **Xử lý dữ liệu:** `pandas`, `numpy`
* **Trực quan hóa:** `matplotlib`, `seaborn`, `plotly`
* **Machine Learning:** `scikit-learn` (Random Forest, Logistic Regression, Metrics)
* **Ensemble Learning & Imbalanced Data:** `xgboost`, `imblearn` (SMOTE)

## 📁 Dữ liệu Đầu vào & Đầu ra
* **Đầu vào:** `Telco_customer_churn.xlsx` (Thông tin của 7.043 khách hàng).
* **Đầu ra:** `Telco_Customer_Churn_Predictions.xlsx` - Tập dữ liệu đã được làm giàu thêm các trường `Churn_Probability`, `Churn_Prediction`, và `Risk_Level` sẵn sàng cho các chiến dịch kinh doanh.

## 💡 Tổng kết Insights & Khuyến nghị
1. **Khủng hoảng tháng đầu tiên:** Giai đoạn rủi ro lớn nhất nằm ở **10 tháng đầu**. Doanh nghiệp cần triển khai các quy trình Onboarding và kiểm tra trải nghiệm gắt gao ngay trong tháng đầu tiên kích hoạt dịch vụ.
2. **Chuyển đổi hợp đồng:** Hợp đồng "Tháng qua tháng" (Month-to-month) là nguyên nhân cốt lõi dẫn đến thiếu cam kết. Cần thiết kế các gói bundle dịch vụ ưu đãi để chuyển đổi nhóm này sang hợp đồng 1 năm.
3. **Chủ động thay vì Bị động:** Hệ thống Machine Learning (XGBoost) giúp doanh nghiệp chủ động nhận diện chính xác 72% khách hàng sắp rời đi, mở ra cơ hội tối ưu hóa dòng tiền và tỷ suất hoàn vốn (ROI) cho các chiến dịch Marketing.
