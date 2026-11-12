# Bài nộp LAB5 — T3 Du lịch · Nhóm [điền tên nhóm]

**Lớp:** L3A · **Bàn / zone:** A5 · **Track:** T3

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Chử Thị Ánh | 2A20603360 | Phụ trách §1–§2: lát cắt, phạm vi và kịch bản rủi ro |
| Nguyễn Thế Cường | 2A202603355 | Phụ trách §3–§4: viết phản hồi AI nguyên văn, bước tiếp theo và người được báo cho 2 ca đắt nhất; xác định mức tự chủ của 5 tool, giải thích theo cái giá khi sai; tổng hợp những điểm nhóm bất đồng. |
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

## §3. Khi AI sai (chặng 3)

### Kịch bản đắt nhất 1 — #2, xác nhận trên thẻ giữ chỗ hết hạn

- **AI nói nguyên văn:** “Chỗ BM 201 cho Nguyễn Minh An và Nguyễn Gia Bình được giữ lúc 15:00 đã hết hạn lúc 15:20. Bây giờ là 15:25, vé chưa đổi. Mình sẽ tra cứu lại số chỗ. Nếu còn đủ chỗ, bạn xem lại thông tin và xác nhận mới nhé.”
- **Người dùng thấy:** thẻ cũ mang trạng thái “Hết hạn”, nút xác nhận bị khoá, thời điểm giữ và hết hạn nằm cạnh chuyến và tên hành khách. Có nút “Tra cứu lại” và “Gặp nhân viên”.
- **Bước tiếp theo:** tự tra cứu lại. Nếu còn đủ chỗ, hiển thị lựa chọn, giữ lại sau khi khách chọn và tạo bản tóm tắt cần duyệt mới. Nếu thiếu chỗ, hỏi khách chọn phương án khác. Xác nhận trên thẻ cũ hết hiệu lực.
- **Ai được báo, khi nào:** báo khách ngay khi hết hạn hoặc khi họ quay lại bấm thẻ cũ. Chuyển nhân viên ngay nếu tool lỗi hoặc khách yêu cầu. Fixture chưa có thời gian chờ nhân viên để cam kết.
- **Quyết định:** chọn tra cứu lại và duyệt lại vì thời hạn 20 phút đã hết. Loại cách chấp nhận xác nhận cũ vì không còn bảo đảm giữ chỗ. Loại cách chuyển nhân viên cho mọi ca hết hạn vì khách vẫn có thể chọn chuyến phù hợp qua tra cứu tự động.
- **Loại lỗi:** lỗi hệ thống nếu giao diện cho phép thẻ hết hạn phát sinh đổi vé. Câu trả lời nêu thời điểm cụ thể, tránh quy trách nhiệm cho khách vì quay lại muộn.
- **HAX/PAIR:** **G11** tại dòng “Giữ 15:00, hết hạn 15:20”; **G9** tại nút “Tra cứu lại” để chọn phương án mới; **G8** tại nút “Gặp nhân viên”.
- **Nếu đã thực hiện bằng dữ liệu cũ:** thông báo kết quả thực tế đã biết, khoá việc đổi tiếp và chuyển nhân viên. Khi kết quả chưa rõ, áp dụng đường xử lý của #3.

### Kịch bản đắt nhất 2 — #3, mất phản hồi sau khi gọi đổi vé

- **AI nói nguyên văn:** “Mình đã gửi yêu cầu đổi sang BM 205 ngày 13/11 lúc 09:30 cho Nguyễn Minh An và Nguyễn Gia Bình, nhưng chưa nhận được kết quả. Mình đang chuyển nhân viên xác minh lần gửi đó. Bạn sẽ thấy trạng thái ‘Đang xác minh kết quả’ trong lúc chờ.”
- **Người dùng thấy:** bản tóm tắt yêu cầu vừa gửi, trạng thái “Đang xác minh kết quả”, nút gửi đổi bị khoá và thông báo chuyển nhân viên. Không hiển thị thành công hay thất bại khi chưa có căn cứ.
- **Bước tiếp theo:** gọi `chuyen_nhan_vien` kèm mã đặt chỗ, chuyến cũ/mới, hành khách, nội dung duyệt, thời điểm gửi và phản hồi/lỗi đã nhận. Dùng mã yêu cầu nếu tool thực sự trả về, không tự tạo mã giao dịch giả làm bằng chứng.
- **Ai được báo, khi nào:** khách được báo ngay khi phát hiện mất phản hồi, nhân viên nhận yêu cầu chuyển tiếp ngay trong cùng luồng. Chưa hứa thời gian xác minh vì track không có SLA.
- **Quyết định:** chọn dừng gửi đổi tiếp và chuyển người. Loại cách tự thử lại vì fixture không mô tả chống lặp. Loại cách tra đặt chỗ rồi khẳng định giao dịch thất bại vì `tra_cuu_dat_cho` chưa được mô tả là nguồn kiểm tra trạng thái giao dịch vừa gửi.
- **Loại lỗi:** lỗi hệ thống/kết nối. Cách nói nhận diện điều đã biết là yêu cầu đã gửi, và phần còn thiếu là kết quả. Khách không cần xác nhận lại để sửa lỗi kết nối.
- **HAX/PAIR:** **G10** tại trạng thái xác minh và chặn gửi tiếp; **G11** tại bản tóm tắt yêu cầu, thời điểm và dữ liệu phản hồi; **PAIR Errors + Graceful Failure** tại chuyển nhân viên kèm lịch sử.
- **Nếu nhân viên xác minh đã đổi:** cập nhật đúng chuyến và quyền đổi đã dùng. Nếu đổi nhầm, nhân viên tiếp tục xử lý theo khả năng thực tế, không hứa hoàn tác vì bộ tool chưa có thao tác đó.

**Lỗi sửa câu chữ không cứu được:** nút xác nhận cũ vẫn gọi được tool sau khi hết hạn, hoặc một thao tác gửi lặp tạo thêm lần đổi khi kết quả trước chưa rõ. Cần chặn tại bước thực thi, gắn duyệt với đúng lựa chọn còn hiệu lực và khoá yêu cầu đang chờ xác minh. Backend triển khai thật cần kiểm tra điều kiện và xử lý gửi lặp, bản mô phỏng giao diện chỉ minh hoạ các chốt này.

## §4. Mức tự chủ  (chặng 4)

| Tool | Mức tự chủ | Cách áp dụng và lý do |
|---|---|---|
| `tra_cuu_dat_cho` | Tự làm | AI được tự tra cứu sau khi khách mở đúng đặt chỗ và hỏi về chuyến. Tool chỉ đọc; nếu phải chờ nhân viên tra hộ, chuyến phù hợp có thể hết chỗ. AI chỉ nói giờ bay và số chỗ lấy từ dữ liệu trả về, không tự suy đoán. |
| `giu_cho_chuyen_moi` | Tự làm có điều kiện | Chỉ giữ chỗ sau khi khách đã chọn rõ chuyến và hành khách. Việc giữ chỗ tự hết hạn sau 20 phút nên có thể đảo lại; chờ nhân viên duyệt có thể làm lỡ chỗ. Không được tự chọn chuyến hoặc hành khách rồi giữ thay khách. |
| `doi_chuyen_bay` | Cần khách duyệt | Trước khi gọi tool, hiển thị chuyến và hành khách cụ thể, nhắc đây là lần đổi miễn phí duy nhất, rồi yêu cầu khách xác nhận rõ. Đổi nhầm khó đảo ngược; một bước xác nhận ngắn giúp chặn lỗi. Nếu khách không xác nhận, không đổi vé. Khi đã gửi mà mất phản hồi, khoá gửi lại và chuyển nhân viên, chưa kết luận thành công hay thất bại. |
| `gui_yeu_cau_hoan_ve` | Cần khách duyệt | Chỉ gửi khi khách chủ động yêu cầu và xác nhận sau khi được báo rằng yêu cầu gửi rồi không huỷ được. Không tự chuyển sang hoàn tiền khi khách đang hỏi đổi chuyến. |
| `chuyen_nhan_vien` | Tự làm có điều kiện | AI tự chuyển khi khách yêu cầu, tool gặp lỗi, khách báo đã đổi nhầm, hoặc tình huống vượt phạm vi như đổi tên/người dưới tuổi bay một mình. Chuyển chat có thể đảo lại; bắt khách chờ thêm một lần duyệt không giúp ích. |

### Tool khó xếp nhất: `giu_cho_chuyen_moi`

**Phương án đối chiếu cho từng tool:** không bắt nhân viên duyệt tra cứu vì chỉ đọc và việc chờ có thể làm lỡ chỗ; không cấm giữ chỗ vì mất cơ hội giữ chuyến phù hợp, nhưng cũng không giữ khi chưa chọn rõ (#4, #5). Không cho đổi vé tự động vì lỗi #2/#3 khó đảo lại; không cấm hoàn toàn việc đổi vì khách đã chọn rõ vẫn cần được xử lý nhanh. Với hoàn vé, không tự gửi vì không rút lại được, nhưng cũng không cấm khi khách thực sự muốn hoàn. Không bắt duyệt lần nữa trước khi chuyển nhân viên vì làm chậm xử lý các ca ngoài thẩm quyền (#6) hoặc kết quả đổi cần xác minh (#3).

**Duyệt và quá hạn:** khách là người duyệt đổi hoặc hoàn. Trước khi hoàn, khách phải thấy chuyến BM 213, hành khách được hoàn, quyền hoàn toàn bộ theo fixture, cảnh báo “Gửi rồi không huỷ được”, tiền về phương thức thanh toán gốc và thời gian có thể tới 90 ngày theo bối cảnh track. Nếu khách chưa xác nhận thì không gửi; việc chờ có thể làm lỡ lựa chọn bay thay thế nhưng không được dùng lý do đó để tự gửi hoàn. Khi chỗ giữ hết 20 phút, mọi xác nhận đổi đang chờ hết hiệu lực, phải tra cứu và duyệt lại. Fixture không quy định hạn duyệt hoàn riêng nên không tự đặt hạn hay tự gửi khi hết giờ. Prototype hiện hỗ trợ hỏi thông tin hoàn vé và hướng dẫn chuyển nhân viên. Bước duyệt và gửi yêu cầu hoàn được mô tả trong thiết kế, chưa triển khai thành thao tác gửi trong demo. trong prototype dự kiến hai màn hình ở §5.

### Các phương án cho `giu_cho_chuyen_moi`

| Phương án | Lợi ích | Cái giá khi sai hoặc chờ |
|---|---|---|
| Tự giữ ngay khi AI tìm được chuyến | Rút ngắn thao tác, giảm nguy cơ hết chỗ trong lúc khách cân nhắc. | AI có thể giữ sai chuyến hoặc hành khách khi khách nói mơ hồ (#4) hay vừa yêu cầu dừng (#5). |
| Cần khách duyệt riêng trước mỗi lần giữ | Khách kiểm tra được chuyến và hành khách trước khi giữ. | Thêm một bước chờ, chỗ có thể hết trước khi giữ thành công (#1). |
| Tự giữ sau khi khách chọn rõ chuyến và hành khách | Giữ được chỗ sớm, đồng thời có căn cứ về lựa chọn của khách. | Phải hiển thị rõ thời hạn và quản lý hiệu lực thẻ giữ để tránh dùng xác nhận cũ (#2). |
| Cấm AI giữ chỗ, chuyển nhân viên | Nhân viên kiểm tra trực tiếp trước khi thao tác. | Khách phải chờ cả trong ca đơn giản, có thể mất chuyến phù hợp. |

**Quyết định của nhóm:** chọn tự làm có điều kiện. AI chỉ giữ sau khi khách chọn rõ chuyến và hành khách, số chỗ đủ và tool trả thành công. Thẻ giữ hiển thị thời hạn 20 phút, trạng thái vé chưa đổi và bước duyệt riêng trước khi đổi vé.

**Lý do chốt:** giữ chỗ có thể tự hết hạn nên chi phí sai thấp hơn đổi vé, nhưng vẫn cần kiểm soát lựa chọn và thời gian. Với #2, thẻ hết hạn phải bị khoá và khách cần duyệt lại sau khi tra cứu. Với #3, giữ chỗ mới không giải quyết được kết quả đổi trước đó đang chưa rõ, nên phải chuyển nhân viên xác minh.

**Ý kiến ban đầu của từng thành viên:** chưa có ghi chép được cung cấp. Nhóm cần bổ sung lựa chọn và lý do thực tế của Chử Thị Ánh, Nguyễn Thế Cường và Hoàng Ngọc Minh Trí.

### Thử mức tự chủ trên ba kịch bản

1. **#2 — khách xác nhận BM 201 cho cả hai sau 25 phút.** AI khoá thẻ giữ đã hết hạn, tự tra cứu lại và hỏi khách chọn/duyệt thông tin mới. Chặn đổi bằng thẻ cũ, chưa làm việc cần duyệt khi chưa có xác nhận mới. Khách tự xử lý được nếu còn chuyến phù hợp, không phải chờ nhân viên chỉ vì hết hạn.
2. **#3 — mất phản hồi sau lệnh đổi BM 205.** AI khoá gửi lại, báo đang xác minh và tự chuyển nhân viên kèm lịch sử. Chặn thao tác lặp trong lúc chưa rõ kết quả. Thời gian chờ là cần thiết vì fixture thiếu tool kiểm tra giao dịch và bảo đảm chống lặp.
3. **Ca bình thường — khách chọn rõ BM 205 cho cả hai, duyệt trong thời hạn giữ, tool trả thành công.** AI tra cứu, giữ 20 phút rồi chỉ gọi đổi sau khi khách duyệt bản tóm tắt. Mỗi yêu cầu được gửi một lần, kết quả thành công mới làm đổi trạng thái. Khách không phải chờ nhân viên.

### Luật bổ sung

- “Tìm chuyến” không đồng nghĩa với “đổi chuyến”.
- Không giữ hoặc đổi chuyến theo suy đoán; phải khớp chuyến và hành khách khách đã chọn.
- Hết 20 phút giữ chỗ thì báo khách, khoá duyệt cũ và tra cứu lại. Lựa chọn mới cần khách duyệt lại (#2).
- Đã gửi lệnh đổi mà mất phản hồi thì khoá gửi tiếp và chuyển nhân viên xác minh (#3). Chỉ một lần đổi đang xử lý cho cùng yêu cầu tại một thời điểm.

### Mức tự chủ đã đổi lời hứa với người dùng

Với `doi_chuyen_bay`, nhóm không hứa “AI tự đổi vé cho bạn”. Lời hứa là: **AI tìm và có thể giữ chuyến phù hợp; khách xem lại chuyến, hành khách và xác nhận trước khi vé được đổi.**

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```
