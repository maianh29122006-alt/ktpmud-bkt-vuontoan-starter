# Yêu cầu dự án - Track B (Ước lượng trước khi tính đúng)

## 1. Phạm vi dự án (Scope)
Mở rộng hệ thống luyện tập hiện có bằng cách thêm dạng bài "Ước lượng kết quả trước khi tính đúng" cho các phép nhân/chia. Học sinh sẽ phải nhập một khoảng hoặc giá trị ước lượng hợp lý vào hệ thống trước khi xem/thực hiện bước tính chính xác. 
Toàn bộ logic kiểm tra đúng/sai phải được xử lý ở phía Server, không sử dụng LLM, và không làm thay đổi các cơ chế xác thực hay hệ thống tài khoản mẫu hiện tại.

## 2. Tiêu chí nghiệm thu (Acceptance Criteria)

* **B-01:** Dạng bài "Ước lượng" phải được tích hợp vào luồng học hiện tại, không phải là một trang demo tĩnh rời rạc.
* **B-02:** Hệ thống có tối thiểu 12 câu hỏi/fixture synthetic cho dạng bài này.
* **B-03:** Quy tắc chấm điểm "ước lượng hợp lý" được mô tả rõ ràng, có tính tất định và có thể test tự động.
* **B-04:** API cấp phát câu hỏi (GET `/api/question-sets/:setId/questions`) KHÔNG ĐƯỢC phép trả về đáp án đúng.
* **B-05:** Server là nơi trực tiếp chấm và lưu kết quả (POST `/api/submissions`). Khi F5/refresh trang, trạng thái bài làm không bị mất.
* **B-06:** Hệ thống cung cấp feedback rõ ràng cho học sinh, phân biệt được thế nào là "ước lượng hợp lý" và "kết quả chính xác".
* **B-07:** Không chỉnh sửa file `server/auth.js`, session, tài khoản mẫu hoặc cơ chế deployment.
* **B-08:** Có bộ test tự động (Automated Tests) bao phủ các trường hợp: normal (đầu vào hợp lệ), boundary (đầu vào ở ranh giới), invalid input (đầu vào rác/chữ) và scoring logic.
* **B-09:** Tích hợp lệnh `npm test`, đảm bảo chạy pass ngay từ clean checkout.
* **B-10:** Bổ sung cấu hình CI chạy `npm ci` và `npm test` tự động khi có Pull Request.
* **B-11:** Không lưu trữ PII/secret thực; chỉ dùng tài khoản synthetic.
* **B-12:** Giao diện hỗ trợ sử dụng bằng bàn phím (Accessibility) cho luồng chính và hiển thị lỗi dễ hiểu.

## 3. Kế hoạch kiểm thử (Test Plan sơ bộ)

| Mã TC | Kịch bản kiểm thử | Dữ liệu đầu vào (Input) | Kết quả mong đợi (Expected) |
|---|---|---|---|
| TC_01 | **Normal input:** Nhập ước lượng hợp lý | Câu hỏi: 48 × 6. Nhập: 300 (vì 50 × 6 = 300) | Server chấm đúng, hiển thị feedback "Ước lượng hợp lý". |
| TC_02 | **Boundary input:** Nhập ở ranh giới | Câu hỏi: 48 × 6. Nhập: giá trị Min/Max của ngưỡng chấp nhận. | Server chấm đúng. |
| TC_03 | **Invalid input:** Nhập ký tự không phải số | Câu hỏi: 48 × 6. Nhập: "abc" | UI báo lỗi "Vui lòng nhập số hợp lệ", không gọi API chấm. |
| TC_04 | **B-04:** Kiểm tra lộ đáp án | Gọi API GET `/api/question-sets/...` | Payload JSON trả về không chứa trường `correct_answer`. |
