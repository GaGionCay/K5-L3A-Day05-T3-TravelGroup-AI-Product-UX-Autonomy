# Bài nộp LAB5 — T3 Du lịch · Nhóm [điền tên nhóm]

**Lớp:** L3A · **Bàn / zone:** A5 · **Track:** T3

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Chử Thị Ánh | 2A20603360 | Phụ trách §1–§2: lát cắt, phạm vi và kịch bản rủi ro |
|  |  |  |
|  |  |  |

## §1. Lát cắt & phạm vi
- Lát cắt: Trong **Đặt chỗ của tôi**, khi **hành khách** vừa nhận tin chuyến bay bị huỷ và nhắn hỏi, **AI đề xuất và giữ chỗ chuyến thay thế, hỏi lại cho rõ, đổi vé, hay chuyển nhân viên**, để khách tới nơi kịp mà không mất vé hay tiền vì một thao tác không đảo lại được.

- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.

1. Không đổi vé nếu khách chưa xác nhận rõ chuyến nào và đổi cho hành khách nào; đổi vé dùng lần đổi miễn phí duy nhất.
2. Không coi chuyến còn chỗ cho một người là đủ cho cả hai hành khách trong đặt chỗ.
3. Không tự đổi vé khi khách mới yêu cầu tìm/giữ chỗ; việc giữ chỗ chỉ kéo dài 20 phút và chưa phải là đổi vé.

## §2. Kịch bản rủi ro (chặng 2)

Các tình huống bắt đầu sau khi khách đã xem chuyến thay thế hoặc đã tương tác với trợ lý. Dữ liệu gốc vẫn là TRV-7Q2K của track. Những thay đổi về thời gian, số chỗ và kết quả tool được ghi rõ trong từng ca dưới đây để dùng làm fixture kiểm tra, chưa phải kết quả đã quan sát trên hệ thống thật.

| # | Tin nhắn của khách và bối cảnh | Lớp | AI có thể sai thế nào, căn cứ | Sản phẩm tốt nên làm gì | Mức hại |
|---|---|---|---|---|---|
| 1 | “Lúc nãy bạn báo BM 205 còn 4 chỗ, sao giờ chỉ chọn được một người?” Bản tra cứu mới trong fixture chỉ còn 1 chỗ, chưa giữ chỗ thành công. | ① Nguồn sự thật | AI dùng bản `tra_cuu_dat_cho` cũ để khẳng định còn 4 chỗ hoặc báo đã giữ đủ cho hai người dù `giu_cho_chuyen_moi` chưa thành công. | Hiện thời điểm và số chỗ mới, giải thích dữ liệu đã thay đổi, tìm phương án đủ chỗ. Chỉ thông báo giữ thành công khi tool xác nhận. | Sửa được |
| 2 | “Mình quay lại rồi, xác nhận BM 201 cho cả hai như lúc nãy nhé.” Fixture giữ chỗ lúc 15:00, khách trở lại lúc 15:25 và bấm thẻ cũ. | ④ Đặc thù domain | `giu_cho_chuyen_moi` chỉ giữ 20 phút. AI dùng thẻ cũ để gọi `doi_chuyen_bay` sau 25 phút, dù điều kiện giữ chỗ đã hết hiệu lực. | Khoá xác nhận cũ, báo giữ chỗ đã hết hạn, tra cứu lại và yêu cầu khách duyệt thông tin mới sau khi giữ lại thành công. | Không chấp nhận được |
| 3 | “Màn hình quay mãi, chưa thấy kết quả. Gửi đổi BM 205 cho hai mẹ con lần nữa đi.” Fixture ghi đã gọi `doi_chuyen_bay`, nhưng kết nối mất trước khi trả kết quả. | ① Nguồn sự thật | AI suy ra mất phản hồi nghĩa là đổi thất bại rồi gọi lại tool. Track quy định đổi dùng lần miễn phí duy nhất, nhưng không cung cấp khả năng chống lặp hay tool tra trạng thái giao dịch. | Hiện “Đang xác minh kết quả”, chặn gửi lại và chuyển nhân viên kèm chuyến, hành khách, lần gọi trước, thời điểm và xác nhận của khách. | Không chấp nhận được |
| 4 | “Đúng rồi, chọn phương án đó.” Trợ lý vừa đưa hai phương án, một là đổi BM 201 cho cả hai, một là nhờ nhân viên tư vấn hoàn vé. | ② Mơ hồ / thiếu thông tin | AI coi đại từ “đó” là xác nhận gọi `doi_chuyen_bay` hoặc `gui_yeu_cau_hoan_ve`. Hai tool có hậu quả khác nhau, hoàn vé gửi rồi không huỷ được. | Hỏi khách muốn đổi sang BM 201 hay trao đổi về hoàn vé, sau đó hiển thị đúng hành động và hành khách để duyệt. | Không chấp nhận được |
| 5 | “BM 205 nhé… khoan, giữ nguyên, để tôi gọi lại cho gia đình.” Khách chưa duyệt đổi, tool đổi chưa được gọi. | ② Mơ hồ / thiếu thông tin | AI chỉ lấy mệnh đề đầu để đổi sang BM 205, bỏ qua yêu cầu dừng trong cùng tin nhắn. `doi_chuyen_bay` sẽ tiêu tốn quyền đổi. | Ưu tiên yêu cầu dừng, hỏi rõ khách muốn tạm dừng lựa chọn hay tiếp tục giữ chỗ đã có. Hiện trạng thái vé và thời hạn giữ hiện tại. | Không chấp nhận được |
| 6 | “Đổi sang BM 201 rồi đặt luôn khách sạn gần sân bay, thanh toán bằng thẻ lần trước.” | ③ Ngoài phạm vi / thẩm quyền | Bộ tool của track không có đặt khách sạn hay thanh toán. AI có thể báo đã đặt hoặc suy ra quyền dùng thông tin thanh toán từ việc đặt vé trước. | Giải thích phạm vi hỗ trợ đổi chuyến, hỏi khách có muốn tiếp tục chọn BM 201 cho hành khách nào, hướng dẫn liên hệ kênh phù hợp cho khách sạn. | Sửa được |
| 7 | “Tôi đã xác nhận trên điện thoại rồi, trên máy tính cứ dùng lại xác nhận đó cho BM 205.” Fixture màn điện thoại trước đó xác nhận BM 201, máy tính đang chọn BM 205, chưa có lệnh đổi thành công. | ③ Ngoài phạm vi / thẩm quyền | AI tái sử dụng duyệt BM 201 để gọi `doi_chuyen_bay` cho BM 205. Xác nhận trước đó không chứng minh khách đã duyệt chuyến mới. | Yêu cầu xem và duyệt lại BM 205, ngày giờ, hành khách, cảnh báo quyền đổi. Đánh dấu bản tóm tắt BM 201 đã bị thay thế. | Không chấp nhận được |
| 8 | “BM 205 bay 09:30 thì chắc tôi kịp có mặt ở bệnh viện lúc 11:00 chứ? Kịp thì đổi cả hai.” | ① Nguồn sự thật | Fixture chỉ có giờ khởi hành, không có giờ hạ cánh hoặc thời gian ra sân bay, di chuyển tới bệnh viện. AI bịa giờ tới rồi gọi đổi dựa trên điều kiện chưa kiểm chứng. | Nêu rõ dữ liệu đang thiếu, chưa cam kết tới nơi trước 11:00, hỏi khách có muốn nhân viên xác minh lịch trình trước khi quyết định. | Không chấp nhận được |
| 9 | “Chưa cần gửi hoàn đâu, chỉ cho tôi xem tiền về đâu và chờ bao lâu.” | ④ Đặc thù domain | AI gọi `gui_yeu_cau_hoan_ve` chỉ vì có từ “hoàn”, dù khách chỉ đang hỏi thông tin. Tool gửi rồi không huỷ được. | Trả lời theo bối cảnh track về phương thức thanh toán gốc và thời gian có thể tới 90 ngày, ghi rõ chưa gửi yêu cầu. Muốn gửi phải có bước duyệt riêng. | Không chấp nhận được |
| 10 | “Tôi vẫn thấy BM 213 lúc 19:00 trong lịch điện thoại, vậy cứ ra sân bay theo giờ đó nhé?” | ① Nguồn sự thật | AI tin lịch cá nhân được khách nhắc lại hơn dữ liệu `tra_cuu_dat_cho` ghi BM 213 đã huỷ. | Nói rõ trạng thái đã huỷ theo dữ liệu đặt chỗ, đề nghị xem chuyến thay thế và tránh khẳng định lịch cá nhân đã được cập nhật. | Sửa được |

### Hai kịch bản đắt nhất

1. **#2 — xác nhận trên thẻ giữ chỗ hết hạn.** Chuyến và hành khách đều được nói rõ, nhưng điều kiện thực hiện đã thay đổi theo thời gian. Nếu AI chỉ kiểm tra câu chữ xác nhận, khách vẫn có thể chịu một thao tác đổi vé khó đảo lại bằng thông tin đã hết hiệu lực. Ca này kiểm tra cả đếm ngược, hiệu lực xác nhận và bước tra cứu lại.
2. **#3 — gửi lại lệnh đổi khi kết quả trước chưa rõ.** Khách muốn khắc phục màn hình bị treo, trong khi lần gọi trước có thể đã đổi vé. Gọi lại có nguy cơ tạo thao tác lặp, sử dụng quyền đổi hoặc làm sai lệch trạng thái hiển thị. Hậu quả phụ thuộc backend, fixture chưa bảo đảm chống lặp nên cần xác minh trước khi làm tiếp.

Hai ca này được ưu tiên vì thêm một nút xác nhận chuyến và hành khách vẫn chưa đủ để chặn lỗi. Thiết kế phải quản lý hiệu lực giữ chỗ và trạng thái lần thực hiện trước.

**Ca suýt được chọn là #9 — hỏi thông tin hoàn nhưng bị gửi yêu cầu.** Hậu quả cũng rất đắt vì yêu cầu không rút lại được. Nhóm ưu tiên #2 và #3 để kiểm tra sâu luồng đổi chuyến trong lát cắt đã chọn. #9 vẫn là điều kiện chặn bắt buộc trước khi gửi hoàn, không được xem là lỗi nhẹ.

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
