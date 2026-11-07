# ADR 0001: Lựa chọn Tech Stack cho Frontend

## 1. Bối cảnh (Context)
Dự án yêu cầu xây dựng một ứng dụng web phục vụ bài tập tương tác Toán lớp 3 (Track B) trong thời gian ngắn (Hackathon 2 tuần). Chức năng cốt lõi là kéo thả (Drag and Drop) thẻ phân số vào vùng nhận tương ứng, kiểm tra tính đúng đắn và xử lý trạng thái bài làm liên tục.
Chúng ta cần chọn một công nghệ Frontend phù hợp để đảm bảo tính năng hoạt động mượt mà, dễ quản lý trạng thái (state) và có thể dựng được một Vertical Slice hoàn chỉnh cho Checkpoint 2.

## 2. Các phương án xem xét (Considered Options)
* **Phương án 1:** HTML/CSS/JavaScript thuần (Vanilla JS) kết hợp HTML5 Drag and Drop API.
* **Phương án 2:** React (có thể dùng kèm thư viện `react-beautiful-dnd` hoặc `@hello-pangea/dnd`).
* **Phương án 3:** Vue.js.

## 3. Quyết định (Decision)
Chúng tôi quyết định chọn **Phương án 2 (React)** làm nền tảng phát triển Frontend chính cho bài toán này.

**Lý do:**
* Việc chia bài toán thành các component nhỏ (như `Card`, `DropZone`, `ScoreBoard`) rất trực quan trong React.
* Quản lý trạng thái (state) khi kéo thả liên tục giữa các vùng chứa sẽ an toàn và ít lỗi đồng bộ (UI out of sync) hơn so với việc tự thao tác trực tiếp lên DOM bằng Vanilla JS.
* Thành viên trong nhóm đã có sẵn kinh nghiệm cấu hình cơ bản với React, giúp rút ngắn thời gian làm quen và bắt tay vào code ngay cho M2.

## 4. Hệ quả (Consequences)
**Tích cực:**
* Dễ dàng tái sử dụng các component nếu muốn mở rộng thêm dạng bài kéo thả khác trong tương lai.
* Rút ngắn thời gian phát triển giao diện tương tác.

**Tiêu cực (Cần lưu ý):**
* Cần kiểm soát chặt chẽ các lượt re-render (render lại) không cần thiết để tránh tình trạng giật lag khi đang kéo thả.
* Phải tuân thủ kỹ việc tách biệt Logic xử lý điểm số và View hiển thị để đáp ứng tốt tiêu chí đánh giá vấn đáp (Viva).
