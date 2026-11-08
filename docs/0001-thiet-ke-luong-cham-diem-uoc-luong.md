# ADR 0001: Thiết kế cơ sở dữ liệu và logic chấm điểm cho dạng bài Ước lượng

## 1. Bối cảnh (Context)
Hệ thống hiện tại có 4 `type` câu hỏi (`multiple-choice`, `input`, `compare`, `paint`) và chấm điểm bằng cách **so sánh chuỗi chính xác** (`given === correct_answer`) tại file `server/api.js`.
Dạng bài mới (Track B) yêu cầu chấm "Ước lượng hợp lý". Điều này đồng nghĩa với việc đáp án hợp lệ là một **khoảng giá trị (min - max)** thay vì một chuỗi tuyệt đối.
Ràng buộc cốt lõi: Không viết lại hệ thống, tận dụng DB schema SQLite thuần hiện có.

## 2. Các phương án xem xét (Considered Options)

* **Phương án 1:** Thêm 2 cột mới `min_value` và `max_value` vào bảng `questions` trong `server/db.js`.
* **Phương án 2:** Thêm một `type` mới là `estimate`. Tái sử dụng cột `correct_answer` hiện tại bằng cách lưu trữ một chuỗi JSON định dạng: `{"min": 250, "max": 350}` hoặc lưu chuỗi phân tách `"250-350"`. Sửa đổi logic tại route `POST /submissions`.

## 3. Quyết định (Decision)
Nhóm quyết định chọn **Phương án 2**: Thêm type `estimate` và lưu chuỗi định dạng `<min>-<max>` (ví dụ: `"250-350"`) trực tiếp vào cột `correct_answer` có sẵn.

Tại `server/api.js`, chúng tôi sẽ bổ sung luồng rẽ nhánh:
```javascript
if (q.type === 'estimate') {
    const [min, max] = q.correct_answer.split('-').map(Number);
    const userValue = Number(given);
    const ok = userValue >= min && userValue <= max;
} else {
    // Luồng so sánh chuỗi chính xác cũ
}
