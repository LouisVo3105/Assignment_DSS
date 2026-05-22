# Dự án Phân tích và Dự báo Khách hàng Rời bỏ (Telco Customer Churn)
## 📌 Giới thiệu dự án
Dự án này tập trung vào việc phân tích tập dữ liệu khách hàng của một công ty viễn thông (Telco) nhằm hiểu rõ các nguyên nhân và đặc điểm của những khách hàng đã ngừng sử dụng dịch vụ (Churn). Từ những hiểu biết đó, dự án xây dựng các mô hình Machine Learning để dự đoán sớm những khách hàng hiện tại có nguy cơ rời bỏ cao, giúp doanh nghiệp kịp thời có các chiến lược chăm sóc và giữ chân khách hàng (Retention).
## 🚀 Các bước thực hiện trong Notebook
Notebook được chia thành 5 phần chính:
1. **Tải dữ liệu và Kiểm tra tổng quan**:
* Kết nối Google Drive và đọc tập dữ liệu từ file Excel (`Telco_customer_churn.xlsx`).
* Kiểm tra thông tin các trường dữ liệu, số lượng dòng (7043 khách hàng ở bang California, Mỹ) và các kiểu dữ liệu.
2. **Tiền xử lý dữ liệu (Data Preprocessing)**:
* Chuyển đổi kiểu dữ liệu cột `Total Charges` về dạng số (Numerical).
* Xử lý giá trị khuyết thiếu (Missing values):
* Loại bỏ cột `Churn Reason` do có tới 73% dữ liệu bị thiếu (tương ứng với những người chưa rời bỏ dịch vụ).
* Điền các giá trị thiếu trong `Total Charges` bằng công thức `Monthly Charges * Tenure Months`.
3. **Phân tích Khám phá Dữ liệu (EDA)**: Phân tích trực quan hóa dữ liệu bằng Plotly và Seaborn để rút ra các insight kinh doanh:
* **Tỉ lệ rời bỏ tổng thể**: Có 26.5% khách hàng trong tập dữ liệu đã ngừng sử dụng dịch vụ.
* **Phân bố theo địa lý**: Los Angeles là thành phố có lượng khách hàng lớn nhất, đồng thời cũng được phân tích để xem tỷ lệ rời bỏ theo từng khu vực.
* **Thời gian gắn bó (Tenure)**: Một nửa số khách hàng rời đi chỉ sau 10 tháng đầu tiên sử dụng dịch vụ.
* **Loại hợp đồng**: Khách hàng sử dụng hợp đồng trả từng tháng (Month-to-month) có tỷ lệ rời bỏ cao nhất (chiếm 43%).
* **Phương thức thanh toán**: Thanh toán bằng "Electronic check" phổ biến nhất nhưng cũng đi kèm tỷ lệ rời bỏ cao nhất.
4. **Xây dựng Mô hình Dự báo (Machine Learning)**:
* Chuẩn bị dữ liệu (Encoding) và xử lý vấn đề mất cân bằng dữ liệu (Imbalanced data) bằng kỹ thuật **SMOTE**.
* Huấn luyện và đánh giá hai mô hình phân loại:
* **XGBoost Classifier**: Độ chính xác (Accuracy) ~81.33%, ROC AUC ~71.93%.
* **Random Forest Classifier**: Độ chính xác (Accuracy) ~80.84%, ROC AUC ~75.80%.
* So sánh dựa trên Precision và Recall để chọn mô hình phù hợp với mục tiêu kinh doanh cụ thể.
5. **Dự báo và Lập danh sách khách hàng có nguy cơ rời bỏ**:
* Sử dụng mô hình (XGBoost) để dự đoán xác suất rời bỏ (`Churn_Probability`) cho toàn bộ tập dữ liệu.
* Lọc ra những khách hàng hiện **chưa rời bỏ** nhưng có xác suất rời bỏ cao (> 0.5) và xếp vào nhóm `High Risk`.
* Đưa ra Top 10 khách hàng cần ưu tiên chăm sóc và xuất kết quả cuối cùng ra file Excel (`Telco_Customer_Churn_Predictions.xlsx`).
## 🛠️ Công cụ và Thư viện sử dụng
* **Ngôn ngữ**: Python
* **Môi trường**: Google Colab / Jupyter Notebook
* **Phân tích và thao tác dữ liệu**: `pandas`, `numpy`
* **Trực quan hóa dữ liệu**: `matplotlib`, `seaborn`, `plotly.express`
* **Machine Learning**: `scikit-learn` (Random Forest, Logistic Regression, Metrics, Train/Test split)
* **Mô hình Gradient Boosting**: `xgboost`
* **Xử lý mất cân bằng dữ liệu**: `imblearn` (SMOTE)
## 📁 Dữ liệu đầu vào và Đầu ra
* **Đầu vào (Input)**: `Telco_customer_churn.xlsx` - chứa thông tin nhân khẩu học, dịch vụ đã đăng ký, chi tiết tài khoản và trạng thái rời bỏ của 7043 khách hàng.
* **Đầu ra (Output)**: `Telco_Customer_Churn_Predictions.xlsx` - Dữ liệu đã được bổ sung thêm các cột `Churn_Probability`, `Churn_Prediction`, và `Risk_Level` để đội ngũ kinh doanh dễ dàng sử dụng.
## 💡 Kết luận / Insights
* Giai đoạn rủi ro lớn nhất của công ty nằm ở **10 tháng đầu tiên** sau khi khách hàng đăng ký dịch vụ.
* **Hợp đồng ngắn hạn (Tháng qua tháng)** mang lại rủi ro rời bỏ cực kỳ cao so với các hợp đồng 1 năm hoặc 2 năm. Công ty nên có chiến dịch ưu đãi để khuyến khích khách hàng chuyển sang hợp đồng dài hạn.
* Mô hình Machine Learning có thể hỗ trợ tốt quá trình chủ động xác định những cá nhân sắp hủy dịch vụ, từ đó tối ưu hóa chi phí Marketing & CSKH.
