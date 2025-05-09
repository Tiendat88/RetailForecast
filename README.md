Dự án Dự đoán Số lượng Bán hàng Thương mại Điện tử
Tổng quan
Dự án này phân tích dữ liệu bán hàng từ bộ dữ liệu OnlineRetail.csv để xây dựng mô hình dự đoán số lượng sản phẩm (Quantity) bán ra. Mục tiêu là hỗ trợ đội ngũ Lập kế hoạch Bán hàng & Vận hành (S&OP) của một công ty thương mại điện tử trong việc chuẩn bị cho các đợt bán hàng cuối năm, quản lý kho hàng và đảm bảo giao hàng đúng hạn.

Dự án bao gồm:

Chia dữ liệu thành tập huấn luyện và kiểm tra dựa trên ngày 25/09/2011.
Xây dựng mô hình hồi quy tuyến tính để dự đoán số lượng bán ra và tính toán Sai số Tuyệt đối Trung bình (MAE).
Dự đoán tổng số lượng sản phẩm bán ra trong tuần 39 năm 2011.
Tính năng
Tiền xử lý dữ liệu:
Xóa các dòng thiếu CustomerID và Description.
Loại bỏ các giao dịch có số lượng âm.
Chuyển đổi cột InvoiceDate sang định dạng ngày.
Mã hóa các cột Country và StockCode bằng LabelEncoder.
Chia dữ liệu: Tách dữ liệu thành tập huấn luyện (trước và bao gồm 25/09/2011) và tập kiểm tra (sau 25/09/2011).
Dự đoán: Sử dụng mô hình hồi quy tuyến tính để dự đoán số lượng bán ra.
Đánh giá: Tính toán MAE trên tập kiểm tra.
Dự báo: Ước tính số lượng sản phẩm bán ra trong tuần 39 năm 2011.
Bộ dữ liệu
File: OnlineRetail.csv
Cột chính:
InvoiceNo: Mã giao dịch (6 chữ số).
StockCode: Mã sản phẩm (5 chữ số).
Description: Tên sản phẩm.
Quantity: Số lượng sản phẩm trong mỗi giao dịch.
InvoiceDate: Ngày giờ giao dịch (MM/DD/YYYY).
UnitPrice: Giá mỗi đơn vị sản phẩm.
CustomerID: Mã khách hàng (5 chữ số).
Country: Quốc gia của khách hàng.
Kích thước: 541,909 dòng, 8 cột.
Yêu cầu
Để chạy dự án, cài đặt các gói Python sau:

bash

Sao chép
pip install pyspark pandas numpy scikit-learn
Lưu ý: Cần cài đặt Apache Spark để sử dụng PySpark. Hướng dẫn cài đặt Spark: Apache Spark Documentation.

Cài đặt
Tải repository hoặc các file dự án.
Đặt file OnlineRetail.csv vào thư mục dự án (hoặc cập nhật đường dẫn trong mã).
Cài đặt các gói phụ thuộc được liệt kê ở phần Yêu cầu.
Đảm bảo môi trường Spark được cấu hình đúng.
Hướng dẫn sử dụng
Mở file Jupyter Notebook final_hw_a39948.ipynb.
Chạy lần lượt tất cả các ô mã để:
Tải và tiền xử lý dữ liệu.
Chia dữ liệu thành tập huấn luyện và kiểm tra.
Mã hóa các cột Country và StockCode.
Huấn luyện mô hình hồi quy tuyến tính.
Tính toán MAE trên tập kiểm tra.
Dự đoán số lượng bán ra trong tuần 39 năm 2011.
Kết quả sẽ được in ra:
pd_daily_train_data: DataFrame chứa dữ liệu huấn luyện.
mae: Sai số tuyệt đối trung bình của mô hình.
quantity_sold_w39: Tổng số lượng sản phẩm dự đoán bán ra trong tuần 39.
Kết quả đầu ra
DataFrame huấn luyện (pd_daily_train_data): Chứa các cột Country, StockCode, InvoiceDate, Quantity.
MAE: Sai số tuyệt đối trung bình của mô hình trên tập kiểm tra (ví dụ: ~13.57).
Số lượng bán ra tuần 39 (quantity_sold_w39): Tổng số lượng sản phẩm dự đoán (kiểm tra dữ liệu có thể cho kết quả 0 nếu không có giao dịch trong tuần 39).
Hạn chế và Cải tiến
Hạn chế:
Mô hình hồi quy tuyến tính đơn giản, có thể không nắm bắt được các mẫu phức tạp trong dữ liệu.
Dữ liệu tuần 39 có thể không đầy đủ, dẫn đến dự đoán không chính xác.
Cải tiến:
Sử dụng các mô hình phức tạp hơn như Random Forest hoặc LSTM để cải thiện độ chính xác.
Thêm các đặc trưng thời gian (tuần, tháng, ngày trong tuần) vào mô hình.
Kiểm tra và xử lý dữ liệu bất thường (outlier) kỹ hơn.