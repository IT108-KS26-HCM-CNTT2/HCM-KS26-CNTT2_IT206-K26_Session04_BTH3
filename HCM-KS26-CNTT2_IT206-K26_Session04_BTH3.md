

## 1. Phân tích bài toán & Ràng buộc kỹ thuật

Khi khách hàng khiếu nại sản phẩm lỗi, nhân viên CSKH thường dễ bị rối hoặc hứa bừa với khách nếu không nắm rõ quy định. Nếu dùng AI mà không có ràng buộc, AI cũng hay mắc lỗi **"tự bịa chính sách"** (ví dụ tự phán cửa hàng cho đổi trả trong 15 ngày dù đề bài không hề nhắc tới).

Vì vậy, em thiết kế một Prompt áp dụng **Chain-of-Thought (Suy luận từng bước)** bắt buộc AI phải tách bạch rõ ràng giữa **Dữ kiện có thật**, **Giả thuyết kỹ thuật** và **Thông tin chưa xác định**, tuyệt đối không tự suy diễn chính sách của cửa hàng trước khi soạn tin nhắn phản hồi.

## 2. Thiết kế Prompt hoàn chỉnh

```
Bạn là chuyên viên phân tích chất lượng dịch vụ khách hàng (QA/CSKH) tại một cửa hàng bán lẻ laptop. Nhiệm vụ của bạn là phân tích khiếu nại của khách hàng một cách khách quan, logic và đề xuất hướng xử lý an toàn.

Hãy suy nghĩ từng bước một (Chain-of-Thought) theo đúng trình tự dưới đây trước khi đưa ra phản hồi cuối cùng.

### RÀNG BUỘC KỸ THUẬT BẮT BUỘC:
1. Tuyệt đối KHÔNG tự giả định hay bịa đặt chính sách đổi trả/hoàn tiền của cửa hàng (vì dữ liệu chưa cung cấp chính sách này).
2. Phân biệt rạch ròi 3 nhóm thông tin: [Dữ kiện thực tế] (khách nói rõ), [Giả thuyết nguyên nhân] (khả năng xảy ra), và [Thông tin chưa xác định] (cần kiểm tra thêm).
3. Không đưa ra kết luận chắc chắn khi chưa có căn cứ kỹ thuật.

### QUY TRÌNH PHÂN TÍCH TỪNG BƯỚC:
- Bước 1: Liệt kê các vấn đề khách hàng đang gặp phải và phân loại thành Dữ kiện / Giả thuyết / Chưa xác định.
- Bước 2: Đánh giá mức độ nghiêm trọng của từng vấn đề (Cao / Trung bình / Thấp) dựa trên trải nghiệm người dùng và rủi ro mất khách.
- Bước 3: Lập danh sách các thông tin còn thiếu cần xác minh (từ hệ thống cửa hàng, bộ phận kỹ thuật và từ khách hàng).
- Bước 4: Đề xuất 2 phương án giải quyết khả thi (không vi phạm quy tắc tự hứa đổi/hoàn tiền ngay), phân tích rõ ưu điểm và hạn chế của từng phương án.
- Bước 5: Soạn thảo mẫu tin nhắn phản hồi gửi khách hàng (thái độ cầu thị, đồng cảm, lịch sự, hẹn hướng xử lý cụ thể mà không hứa suông).

### NỘI DUNG KHIẾU NẠI CỦA KHÁCH HÀNG:
"{{NOI_DUNG_KHIEU_NAI}}"

```

## 3. Kết quả chạy thử thực tế (Đầu ra của AI)

Với nội dung khiếu nại của khách hàng mua laptop 10 ngày bị nóng, quạt kêu to, pin 2 tiếng và từng vệ sinh máy nhưng không hết lỗi, AI trả về kết quả theo đúng cấu trúc yêu cầu như sau:

### Báo cáo phân tích theo từng bước (Chain-of-Thought)

#### Bước 1: Xác định vấn đề và Phân loại thông tin

* **Các vấn đề khách hàng đang gặp phải:**
  1. *Lỗi phần cứng/hiệu năng:* Máy thường xuyên nóng, quạt tản nhiệt kêu to, thời lượng pin ngắn (khoảng 2 tiếng).
  2. *Lỗi quy trình dịch vụ:* Khách đã mang máy đi kiểm tra nhưng nhân viên chỉ vệ sinh qua loa, sau 2 ngày lỗi tái diễn.
  3. *Yêu cầu của khách:* Đòi đổi máy mới hoặc hoàn tiền.

* **Phân tách rõ ràng bản chất thông tin:**
  * **[Dữ kiện thực tế]:** Khách mua máy cách đây 10 ngày; đã từng mang máy tới cửa hàng kiểm tra và được vệ sinh; lỗi lặp lại sau 2 ngày; khách đang yêu cầu đổi máy hoặc hoàn tiền.
  * **[Giả thuyết nguyên nhân]:** Máy nóng và pin tụt nhanh có thể do lỗi phần cứng từ nhà sản xuất (pin chai, lỗi tản nhiệt), HOẶC cũng có thể do khách chạy tác vụ nặng/chơi game/để phần mềm chạy ngầm liên tục. Nhân viên kỹ thuật lần trước có thể đã chẩn đoán sơ sài.
  * **[Thông tin chưa xác định]:** Mẫu mã/cấu hình máy là gì (laptop văn phòng hay laptop gaming vì pin 2 tiếng có thể bình thường với máy gaming); phiếu tiếp nhận bảo hành lần trước ghi gì; chính sách đổi trả/hoàn tiền cụ thể của cửa hàng hiện tại ra sao.

#### Bước 2: Phân tích mức độ nghiêm trọng của từng vấn đề

1. **Vấn đề trải nghiệm dịch vụ (Nhân viên chỉ vệ sinh máy rồi trả lại): Mức độ CAO.**
   * *Lý do:* Máy mới mua 10 ngày mà kỹ thuật chỉ vệ sinh (thường chỉ áp dụng cho máy dùng lâu bụi bẩn) khiến khách cảm thấy bị coi thường và thiếu chuyên nghiệp, làm bùng phát tâm lý đòi hoàn tiền.

2. **Vấn đề máy nóng, quạt kêu to, pin 2 tiếng: Mức độ TRUNG BÌNH - CAO.**
   * *Lý do:* Ảnh hưởng trực tiếp đến công việc hàng ngày của khách. Tuy nhiên, cần xác định dòng máy và tác vụ sử dụng mới biết đây là lỗi phần cứng thực sự hay đặc tính của máy.

3. **Yêu cầu đổi máy / hoàn tiền: Mức độ CAO (Rủi ro xung đột).**
   * *Lý do:* Nếu xử lý chậm trễ hoặc từ chối cứng nhắc ngay lập tức, khách hàng rất dễ đánh giá tiêu cực công khai trên mạng xã hội.

#### Bước 3: Danh sách thông tin còn thiếu cần xác minh

* **Xác minh nội bộ (Hệ thống & Bộ phận kỹ thuật):**
  1. Kiểm tra chính sách đổi trả/hoàn tiền hiện hành của cửa hàng áp dụng cho mốc thời gian mua hàng 10 ngày (điều kiện đổi mới là gì, có cần biên bản xác nhận lỗi từ hãng không).
  2. Tra cứu lịch sử tiếp nhận máy lần trước trên hệ thống: Tên nhân viên kỹ thuật nào đã xử lý, đã test những bài kiểm tra gì hay chỉ vệ sinh bên ngoài.
  3. Kiểm tra thông số kỹ thuật chuẩn của dòng laptop khách mua (dung lượng pin thiết kế, mức tản nhiệt tiêu chuẩn).

* **Xác minh từ phía khách hàng:**
  4. Mã đơn hàng / Số điện thoại mua hàng để đối chiếu thời gian mua chính xác.
  5. Khách thường sử dụng các phần mềm/tác vụ gì khi máy bị nóng và tụt pin (lướt web cơ bản hay render video, chơi game, vừa sạc vừa dùng).

#### Bước 4: Đề xuất 2 phương án xử lý (Kèm ưu điểm & hạn chế)

* **Phương án 1: Mời khách mang máy đến kiểm tra kỹ thuật chuyên sâu bởi Trưởng bộ phận Kỹ thuật (Ưu tiên xử lý nhanh và minh bạch)**
  * *Nội dung:* Xin lỗi khách về trải nghiệm lần trước, mời khách mang máy qua để kỹ thuật trưởng kiểm tra toàn diện (test phần cứng, đo nhiệt độ, đo độ chai pin) trước mặt khách để có biên bản xác nhận tình trạng thực tế, từ đó áp dụng đúng quyền lợi bảo hành/đổi trả theo quy định cửa hàng.
  * *Ưu điểm:* Thể hiện sự tôn trọng khách hàng, tránh hứa bừa chính sách đổi/hoàn tiền khi chưa kiểm tra máy; thu thập được bằng chứng kỹ thuật khách quan để làm việc với hãng hoặc giải thích thuyết phục cho khách.
  * *Hạn chế:* Khách hàng có thể khó chịu vì phải mất công đi lại cửa hàng thêm lần nữa sau lần đầu bị xử lý sơ sài.

* **Phương án 2: Hỗ trợ nhận máy kiểm tra tận nơi (hoặc mượn máy dùng tạm trong lúc kiểm tra)**
  * *Nội dung:* Nếu khách ở xa hoặc bận công việc không tiện đi lại lần 2, cửa hàng cử nhân viên đến nhận máy về kiểm tra chuyên sâu, đồng thời cung cấp 1 laptop dùng tạm (nếu cửa hàng có sẵn máy demo) để không gián đoạn công việc của khách trong lúc chờ kết quả thẩm định lỗi.
  * *Ưu điểm:* Xoa dịu cơn giận của khách rất tốt nhờ sự chăm sóc tận tình, giải quyết được nỗi lo mất công đi lại của khách.
  * *Hạn chế:* Tốn thêm chi phí và nhân lực vận chuyển cho cửa hàng; phụ thuộc vào việc cửa hàng có sẵn máy dự phòng cho khách mượn hay không.

#### Bước 5: Mẫu tin nhắn phản hồi chuyên nghiệp gửi khách hàng

> *"Chào anh/chị [Tên khách hàng],*
>
> *Cửa hàng chân thành xin lỗi anh/chị vì sự bất tiện khi chiếc laptop mới mua đã gặp tình trạng nóng máy, quạt kêu to và pin sụt nhanh, đặc biệt là trải nghiệm chưa trọn vẹn trong lần kiểm tra trước đó khiến vấn đề chưa được xử lý dứt điểm.*
>
> *Cửa hàng hoàn toàn thấu hiểu sự lo lắng của anh/chị và ghi nhận mong muốn hỗ trợ đổi/trả máy. Để đảm bảo quyền lợi tốt nhất cho anh/chị theo đúng quy định bảo hành của cửa hàng và hãng, bộ phận Quản lý Kỹ thuật rất mong được kiểm tra chuyên sâu lại tình trạng phần cứng cũng như các tác vụ sử dụng trên máy để có đánh giá chính xác nhất.*
>
> *Anh/chị có thể cho cửa hàng xin mã đơn hàng (hoặc SĐT mua hàng) và khung giờ thuận tiện để kỹ thuật trưởng trực tiếp tiếp nhận kiểm tra ưu tiên cho mình (hoặc hỗ trợ nhận máy tận nơi nếu anh/chị bận) được không ạ?*
>
> *Cửa hàng cam kết sẽ theo sát và xử lý minh bạch trường hợp này cho anh/chị trong thời gian sớm nhất. Rất mong nhận được phản hồi từ anh/chị!"*
