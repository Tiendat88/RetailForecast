# RetailForecast — Dự báo nhu cầu bán lẻ trực tuyến

Bài tập cuối kỳ môn Big Data: phân tích bộ dữ liệu giao dịch thương mại điện tử
`OnlineRetail.csv` và xây dựng mô hình dự báo số lượng sản phẩm bán ra (`Quantity`),
phục vụ cho đội **Sales & Operations Planning (S&OP)** lập kế hoạch tồn kho
và khuyến mãi cuối năm.

Xử lý dữ liệu bằng **PySpark**, huấn luyện mô hình bằng **scikit-learn**.

---

## Đề bài

Notebook trả lời 3 câu hỏi:

| # | Yêu cầu | Biến kết quả |
|---|---------|--------------|
| 1 | Tách dữ liệu theo mốc `2011-09-25` (≤ ngày này là train, sau đó là test). Trả về pandas DataFrame gồm ít nhất các cột `Country`, `StockCode`, `InvoiceDate`, `Quantity` | `pd_daily_train_data` |
| 2 | Tính **Mean Absolute Error (MAE)** của mô hình dự báo `Quantity` trên tập test | `mae` (float) |
| 3 | Dự báo tổng số đơn vị bán ra trong **tuần 39 năm 2011** | `quantity_sold_w39` (int) |

## Kết quả hiện tại

| Chỉ số | Giá trị |
|--------|---------|
| Số dòng gốc | 541,909 |
| Số dòng sau khi làm sạch (train) | 257,815 |
| MAE | **13.566** |
| `quantity_sold_w39` | **0** (xem phần [Hạn chế đã biết](#hạn-chế-đã-biết)) |
| Tổng lượng bán tuần 38 + 40 (đối chiếu) | 163,656 |

---

## Dữ liệu

`OnlineRetail.csv` — **không có trong repo**, bạn cần tự chuẩn bị (bộ dữ liệu
Online Retail của UCI Machine Learning Repository). File gồm 8 cột:

| Cột | Mô tả |
|-----|-------|
| `InvoiceNo` | Mã hóa đơn, 6 chữ số, duy nhất theo giao dịch |
| `StockCode` | Mã sản phẩm, 5 ký tự |
| `Description` | Tên sản phẩm |
| `Quantity` | Số lượng sản phẩm trong giao dịch |
| `InvoiceDate` | Thời điểm giao dịch, định dạng chuỗi `M/d/yyyy H:mm` |
| `UnitPrice` | Đơn giá |
| `CustomerID` | Mã khách hàng, 5 chữ số |
| `Country` | Quốc gia của khách hàng |

### Chất lượng dữ liệu

Kiểm tra giá trị thiếu trên dữ liệu gốc:

- `Description`: 1,454 dòng thiếu
- `CustomerID`: 135,080 dòng thiếu
- Các cột còn lại: đầy đủ

Có một số dòng trùng lặp hoàn toàn, nhưng đều là các giao dịch hợp lý
(cùng hóa đơn mua lặp một sản phẩm) nên **được giữ nguyên**.

---

## Luồng xử lý

1. **Đọc dữ liệu** — `SparkSession` đọc CSV với `inferSchema=True`.
2. **Làm sạch** — `dropna` trên `Description` và `CustomerID`; kiểm tra lại còn 0 giá trị null.
3. **Chuẩn hóa ngày** — `to_date("InvoiceDate", "M/d/yyyy H:mm")` đổi `string` → `date`.
4. **Tách tập** — lọc theo `InvoiceDate <= "2011-09-25"` (train) và `> "2011-09-25"` (test),
   chỉ giữ 4 cột `Country`, `StockCode`, `InvoiceDate`, `Quantity`, rồi `.toPandas()`.
5. **Lọc trả hàng** — bỏ các dòng có `Quantity < 0` (đơn hủy/trả hàng).
6. **Mã hóa đặc trưng** — `LabelEncoder` cho `Country` và `StockCode`.
7. **Huấn luyện** — `LinearRegression` với 2 đặc trưng `Country_encoded`, `StockCode_encoded`.
8. **Đánh giá** — `mean_absolute_error(y_test, y_pred)`.
9. **Dự báo tuần 39** — lọc theo `dt.isocalendar().week == 39` và `dt.year == 2011`, cộng `Quantity`.

---

## Cấu trúc repo

```
.
├── final_hw_a39948.ipynb   # Toàn bộ phân tích và mô hình
├── requirements.txt        # Thư viện Python cần thiết
├── README.md               # File này
└── OnlineRetail.csv        # (không kèm theo — tự chuẩn bị)
```

---

## Cài đặt

Yêu cầu: **Python 3.8+**, **Java 8/11/17** (bắt buộc cho Spark).

```bash
git clone https://github.com/Tiendat88/RetailForecast.git
cd RetailForecast

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
pip install jupyter
```

`requirements.txt` gồm: `pyspark`, `pandas`, `numpy`, `scikit-learn`.
PySpark cài qua pip đã kèm sẵn Spark, **không cần** cài Apache Spark riêng —
chỉ cần Java có trong `PATH` (kiểm tra bằng `java -version`).

## Chạy

```bash
jupyter notebook final_hw_a39948.ipynb
```

> **Quan trọng:** sửa biến `file_path` ở cell đọc dữ liệu cho đúng máy bạn.
> Notebook đang hard-code đường dẫn tuyệt đối của máy tác giả:
>
> ```python
> file_path = "/Users/tiendat02/Documents/big-data/Final HW-Demand Forecasting/OnlineRetail.csv"
> ```
>
> Nên đổi thành đường dẫn tương đối: `file_path = "OnlineRetail.csv"`.

Chạy tuần tự các cell từ trên xuống.

---

## Hạn chế đã biết

**1. Tập test đang bị gán nhầm bằng tập train.** Ở cell tạo `pd_daily_test_data`:

```python
pd_daily_test_data = train_df.toPandas()   # ❌ phải là test_df.toPandas()
```

Đây là nguyên nhân trực tiếp của hai vấn đề:

- `mae = 13.566` thực chất là sai số **trên chính tập huấn luyện**, không phải
  sai số ngoài mẫu — con số này lạc quan hơn thực tế.
- `quantity_sold_w39 = 0` vì tuần 39/2011 (26/09 – 02/10) nằm **hoàn toàn sau**
  mốc chia `2011-09-25`, nên không có dòng nào trong tập train rơi vào tuần đó.
  Cell kiểm tra tuần 38 và 40 trả về 163,656 đơn vị, xác nhận dữ liệu vẫn còn —
  chỉ là đang lọc nhầm tập.

Sửa một dòng này sẽ cho MAE ngoài mẫu đúng nghĩa và một con số tuần 39 khác 0.

**2. Mô hình quá đơn giản.** Hồi quy tuyến tính trên hai nhãn đã `LabelEncode`
coi mã quốc gia/sản phẩm như biến liên tục có thứ tự, trong khi chúng chỉ là
nhãn danh định — mô hình gần như không học được quan hệ có ý nghĩa. Hướng cải thiện:

- Dùng one-hot encoding thay vì label encoding, hoặc mô hình cây
  (Random Forest, XGBoost) vốn xử lý được biến danh định.
- Thêm đặc trưng thời gian: tháng, tuần, thứ trong tuần, kỳ nghỉ lễ.
- Tổng hợp dữ liệu theo ngày/sản phẩm rồi dùng mô hình chuỗi thời gian
  (ARIMA, Prophet) — phù hợp hơn với bài toán dự báo nhu cầu.

**3. `LabelEncoder.transform` sẽ lỗi với nhãn mới.** Sau khi sửa lỗi (1),
nếu tập test chứa `StockCode` hoặc `Country` chưa từng xuất hiện trong train,
`transform` ném `ValueError`. Cần lọc bỏ hoặc gán một mã "unknown" cho các nhãn này.

---

## Xử lý sự cố

| Triệu chứng | Cách xử lý |
|-------------|------------|
| `FileNotFoundError` / `Path does not exist` | Sửa `file_path` trỏ đúng vị trí `OnlineRetail.csv` |
| `JAVA_HOME is not set` | Cài JDK (8/11/17) và đặt biến môi trường `JAVA_HOME` |
| `Py4JJavaError` khi khởi tạo Spark | Kiểm tra phiên bản Java tương thích với PySpark đang dùng |
| Kết quả tuần 39 bằng 0 | Xem [Hạn chế đã biết](#hạn-chế-đã-biết) mục 1 |
| MAE cao | Mô hình tuyến tính hạn chế — xem mục 2 |

---

## Giấy phép

Dự án phục vụ mục đích học tập, cung cấp nguyên trạng, không kèm bảo đảm.
