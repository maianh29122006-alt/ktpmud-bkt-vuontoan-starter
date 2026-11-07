# Yêu cầu dự án - Bài kiểm tra Track B (Vườn Toán Lớp 3)

## 1. Phạm vi dự án (Scope)
Xây dựng một dạng bài tập tương tác Toán lớp 3: **Kéo thả thẻ phân số**.
Học sinh sẽ được cung cấp 3 thẻ chữ (chứa các phân số 1/2, 1/3, 1/4) và 3 hình vẽ minh họa (ví dụ: hình tròn chia 2 phần tô màu 1 phần, chia 3 phần tô màu 1 phần...). Nhiệm vụ của học sinh là kéo thẻ phân số tương ứng thả vào ô nhận đáp án bên dưới mỗi hình.

## 2. Tiêu chí nghiệm thu (Acceptance Criteria)

**AC1: Hiển thị giao diện bài tập**
* **Given** học sinh mở bài tập kéo thả phân số "Một phần mấy".
* **When** màn hình tải xong hoàn toàn.
* **Then** hệ thống phải hiển thị ngẫu nhiên 3 hình minh họa và 3 thẻ đáp án (1/2, 1/3, 1/4) không theo thứ tự khớp nhau, cùng với nút "Nộp bài" bị làm mờ (disabled).

**AC2: Tương tác kéo và thả thẻ đáp án**
* **Given** học sinh đang làm bài tập.
* **When** học sinh kéo một thẻ đáp án (VD: "1/2") và thả vào vùng nhận của một hình vẽ.
* **Then** thẻ đáp án phải khớp dính vào vị trí bên dưới hình vẽ đó. Nếu vùng nhận đã có sẵn thẻ, thẻ cũ sẽ bị đẩy trở lại khay chứa ban đầu.

**AC3: Nộp bài và tính điểm (Luồng thành công)**
* **Given** học sinh đã thả đủ 3 thẻ đáp án vào 3 hình vẽ.
* **When** học sinh nhấn nút "Nộp bài".
* **Then** hệ thống khóa chức năng kéo thả, bôi màu xanh cho các thẻ đặt đúng vị trí, hiển thị thông báo "Chúc mừng" và cập nhật điểm số vào database.

**AC4: Nộp bài và phản hồi lỗi (Luồng thất bại)**
* **Given** học sinh đã thả đủ 3 thẻ nhưng có ít nhất 1 thẻ bị sai vị trí.
* **When** học sinh nhấn "Nộp bài".
* **Then** hệ thống bôi màu đỏ cho các thẻ sai, giữ nguyên màu hoặc bôi xanh thẻ đúng, và trừ đi số lượt làm bài còn lại.

## 3. Kế hoạch kiểm thử (Test Plan)

| Mã TC | Tên kịch bản | Các bước thực hiện (Steps) | Kết quả mong đợi (Expected Output) |
|---|---|---|---|
| **TC_01** | Kéo thả hợp lệ | 1. Dùng chuột/cảm ứng kéo thẻ "1/3".<br>2. Thả vào vùng nhận của hình chia 3 phần. | Thẻ "1/3" nằm gọn trong vùng nhận. Không bị văng ra ngoài. |
| **TC_02** | Thay đổi đáp án | 1. Kéo thẻ "1/4" thả vào vùng đã có thẻ "1/3". | Thẻ "1/4" chiếm chỗ, thẻ "1/3" tự động quay về khay chứa ban đầu. |
| **TC_03** | Khóa nút nộp bài | 1. Chỉ kéo 2 thẻ vào hình, để trống 1 hình. | Nút "Nộp bài" ở trạng thái mờ (disabled) và không thể bấm được. |
| **TC_04** | Nộp bài đúng 100% | 1. Kéo chính xác cả 3 thẻ vào 3 hình.<br>2. Bấm "Nộp bài". | Hiện hiệu ứng chúc mừng, ghi nhận điểm lên hệ thống, thẻ đổi sang viền xanh. |
| **TC_05** | Nộp bài sai | 1. Kéo sai vị trí 2 thẻ.<br>2. Bấm "Nộp bài". | Hiện viền đỏ ở 2 thẻ sai. Có nút "Làm lại" hiện ra để học sinh thử lại. |
