---

id: morning-cleaning-sprint
summary: Sprint 45 phút để làm sạch dữ liệu “độc hại” trước khi đưa vào AI.
authors: Vo Tu Duc
categories: Data Engineering
status: Published
-----------------

# Morning Mini-Lab: Sprint Làm Sạch Dữ Liệu

## 1. Giới thiệu: “Mẫu dữ liệu độc hại”

Thời lượng: 0:05:00

Trong thư mục `morning_v2/`, bạn sẽ tìm thấy file `toxic_sample.json`. Dữ liệu này được gọi là “độc hại” vì nó chứa:

* **PII (Personally Identifiable Information - Thông tin định danh cá nhân)**: Tên thật và email thật.
* **Zombies**: Các bản ghi bị trùng lặp.
* **Extreme Outliers**: Giá trị giá quá bất thường, không hợp lý.
* **Garbage**: Giá âm và thiếu danh mục sản phẩm.

---

## 2. Thử thách 1: Ẩn thông tin PII (Privacy)

Thời lượng: 0:15:00

AI Agent của bạn không cần biết tên thật hoặc email thật của khách hàng để trả lời các câu hỏi về sản phẩm. Trên thực tế, việc lưu các dữ liệu này trong Vector DB là một **rủi ro bảo mật**.

### Nhiệm vụ

Viết một script để:

1. **Xóa hoàn toàn** trường `name`.
2. **Ẩn một phần** trường `email`, ví dụ:
   `vana@gmail.com` → `v***@gmail.com`

```python
import json

def mask_email(email):
    parts = email.split('@')
    return parts[0][0] + "***@" + parts[1]

# TODO: Áp dụng cho toxic_sample.json
```

---

## 3. Thử thách 2: Loại bỏ trùng lặp & giá trị ngoại lai

Thời lượng: 0:15:00

AI Agent có thể bị nhầm lẫn bởi dữ liệu trùng lặp và các giá trị bất thường.

### Nhiệm vụ

1. **Loại bỏ trùng lặp**: Đảm bảo mỗi `id` chỉ xuất hiện một lần.
2. **Kiểm tra giá trị ngoại lai**:

   * Một cây bút có giá $99,999 rất có thể là lỗi dữ liệu.
   * Đặt một ngưỡng, ví dụ: $5,000, và loại bỏ mọi item có giá cao hơn ngưỡng này.
3. **Kiểm tra tính hợp lý**: Loại bỏ mọi item có `price < 0`.

---

## 4. Kiểm tra cuối cùng

Thời lượng: 0:10:00

### Sản phẩm cần nộp

Script của bạn cần xuất ra file `sanitized_sample.json` với các yêu cầu sau:

* **Riêng tư**: Không còn tên thật, email đã được ẩn một phần.
* **Gọn nhẹ**: Không còn bản ghi trùng lặp.
* **Thực tế**: Không còn bút chì giá $99,999 hoặc điện thoại có giá âm.

### Câu hỏi thảo luận

“Tại sao chúng ta sử dụng **ETL** — làm sạch dữ liệu trước khi lưu trữ — cho việc ẩn PII, thay vì dùng **ELT** — làm sạch dữ liệu sau khi lưu trữ?”

*Gợi ý: Hãy nghĩ xem dữ liệu PII thô sẽ nằm ở đâu nếu chúng ta dùng ELT.*
