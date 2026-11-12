# Bài nộp LAB5 — T3 Du lịch · Nhóm [điền tên nhóm]

**Lớp:** L3A · **Bàn / zone:** A5 · **Track:** T3

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Chử Thị Ánh | 2A20603360 | Phụ trách §1–§2: lát cắt, phạm vi và kịch bản rủi ro |
|  |  |  |
|  |  |  |

## §1. Lát cắt và phạm vi

Trong Đặt chỗ của tôi, hành khách vừa nhận tin BM 213 bị huỷ do thời tiết và hỏi cách đổi chuyến. AI tra cứu, đề xuất hoặc giữ chỗ chuyến thay thế, hỏi lại khi thiếu thông tin và chỉ đổi vé sau khi khách duyệt rõ chuyến cùng hành khách.

Dữ liệu chính:
- Đặt chỗ TRV-7Q2K có Nguyễn Minh An và Nguyễn Gia Bình, 5 tuổi.
- BM 213 Hà Nội → TP.HCM, 12/11 lúc 19:00, đã huỷ do thời tiết.
- BM 217 lúc 21:00 còn 1 chỗ.
- BM 201 ngày 13/11 lúc 07:00 còn 9 chỗ.
- BM 205 ngày 13/11 lúc 09:30 còn 4 chỗ.
- Được đổi miễn phí một lần trong 48 giờ hoặc hoàn toàn bộ.
- Giữ chỗ kéo dài 20 phút.

Ba việc tính năng không làm:
1. Không đổi vé khi chưa xác nhận rõ chuyến và hành khách vì thao tác dùng lần đổi miễn phí duy nhất.
2. Không coi một chỗ còn lại là đủ cho hai người vì có thể khiến phương án di chuyển không thực hiện được.
3. Không coi yêu cầu tìm hoặc giữ chỗ là đồng ý đổi vé vì giữ chỗ và đổi vé có hậu quả khác nhau.

## §2. Mười kịch bản rủi ro

| # | Tình huống | Lớp | Nguy cơ và cách xử lý |
|---|---|---|---|
| 1 | BM 205 trước còn 4 chỗ, tra lại chỉ còn 1. | Nguồn sự thật | AI dùng dữ liệu cũ. Phải hiện số chỗ và thời điểm mới, chưa báo giữ thành công khi tool chưa xác nhận. |
| 2 | Khách quay lại sau 25 phút và xác nhận thẻ giữ BM 201 cũ. | Đặc thù domain | Thẻ giữ chỉ có hạn 20 phút. Khoá xác nhận cũ, tra lại và yêu cầu duyệt mới. |
| 3 | Đã gửi đổi BM 205 nhưng mất phản hồi, khách yêu cầu gửi lại. | Nguồn sự thật | Lần đầu có thể đã thành công. Khoá gửi lại, báo đang xác minh và chuyển nhân viên. |
| 4 | Khách nói “chọn phương án đó” sau khi được đưa lựa chọn đổi hoặc tư vấn hoàn. | Mơ hồ | AI có thể chọn sai hành động. Hỏi rõ muốn đổi hay tư vấn hoàn trước khi thực hiện. |
| 5 | Khách nói “BM 205 nhé… khoan, giữ nguyên”. | Mơ hồ | AI bỏ qua yêu cầu dừng. Ưu tiên tạm dừng, hiển thị trạng thái và hỏi lại khi cần. |
| 6 | Khách yêu cầu đổi chuyến, đặt khách sạn và thanh toán bằng thẻ cũ. | Ngoài phạm vi | Không có tool khách sạn hoặc thanh toán. Giải thích phạm vi, hướng dẫn kênh phù hợp và hỏi tiếp về chuyến bay. |
| 7 | Khách muốn dùng xác nhận BM 201 trên điện thoại cho BM 205 trên máy tính. | Ngoài thẩm quyền | Xác nhận cũ không áp dụng cho chuyến mới. Phải duyệt lại đúng chuyến và hành khách. |
| 8 | Khách chỉ đồng ý đổi nếu kịp tới bệnh viện lúc 11:00. | Nguồn sự thật | Thiếu giờ hạ cánh và thời gian di chuyển. Nêu giới hạn, chưa cam kết và đề nghị xác minh lịch trình. |
| 9 | Khách chỉ hỏi tiền hoàn về đâu, chờ bao lâu. | Đặc thù domain | AI có thể gửi hoàn ngoài ý muốn. Chỉ trả lời thông tin, ghi rõ chưa gửi yêu cầu. |
| 10 | Lịch điện thoại vẫn hiện BM 213 lúc 19:00. | Nguồn sự thật | AI tin lịch cũ hơn dữ liệu đặt chỗ. Phải thông báo BM 213 đã huỷ và đưa chuyến thay thế. |

Mức hại:
- #1, #6, #10: sửa được.
- #2, #3, #4, #5, #7, #8, #9: không chấp nhận được.

## Hai ca đắt nhất

### #2 — Xác nhận sau khi giữ chỗ hết hạn
Khách đã nói rõ chuyến và hành khách nhưng điều kiện thực hiện đã hết hiệu lực. Một nút xác nhận đơn thuần chưa đủ, hệ thống cần kiểm tra thời hạn, khoá thẻ cũ và tra cứu lại.

### #3 — Mất phản hồi sau khi gửi đổi
Mất phản hồi chưa chứng minh giao dịch thất bại. Gửi lại có thể tạo thao tác lặp trong khi lần đổi miễn phí đã được dùng. Track chưa có tool kiểm tra giao dịch hoặc bảo đảm chống lặp nên phải chuyển nhân viên xác minh.

### Ca suýt được chọn — #9
Gửi hoàn ngoài ý muốn rất nghiêm trọng vì không rút lại được. Nhóm ưu tiên #2 và #3 để tập trung thiết kế luồng đổi chuyến, đồng thời vẫn giữ bước duyệt riêng trước khi gửi hoàn.

## Câu cần giải thích khi demo
- Vì sao hai ca chính nguy hiểm dù khách đã nói rõ chuyến?
- Dữ liệu nào chứng minh thẻ giữ hết hạn?
- Vì sao mất phản hồi không đủ để kết luận đổi thất bại?

## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```
