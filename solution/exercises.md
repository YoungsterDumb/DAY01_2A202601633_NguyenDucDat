# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng mẫu bằng câu trả lời thật (chấm tự
động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature thấp như 0, phản hồi thường ổn định, ít biến hóa và đi
> thẳng vào một sự thật phổ biến. Khi tăng lên 0.5 hoặc 1.0, câu trả lời tự
> nhiên và đa dạng hơn; đến 1.5 thì model có xu hướng sáng tạo hơn nhưng cũng
> dễ lan man 

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Đặt khoảng từ 0.2 đến 0.6 cho chatbot hỗ trợ khách hàng. Mức này giúp câu
> trả lời nhất quán, dễ kiểm soát và ít bịa thông tin, nhưng vẫn đủ tự nhiên để
> người dùng không cảm thấy quá máy móc.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Kịch bản có 10.000 x 3 x 350 = 10.500.000 token đầu ra mỗi ngày. Với giá đầu
> ra trong bảng, GPT-4o khoảng 105 USD/ngày, còn GPT-4o-mini khoảng 6,3
> USD/ngày, tức GPT-4o đắt hơn khoảng 16,7 lần. GPT-4o xứng đáng khi cần phân
> tích phức tạp, độ chính xác và lập luận cao, ví dụ trợ lý tư vấn kỹ thuật
> chuyên sâu; GPT-4o-mini phù hợp cho FAQ, tóm tắt ngắn, phân loại yêu cầu hoặc
> chatbot khối lượng lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên tiểu học, phản hồi thường ngắn hơn, dùng từ đơn giản và
> ví dụ gần gũi như cuốn sổ ghi chép chung. Với persona chuyên gia tài chính,
> câu trả lời dài hơn, dùng nhiều thuật ngữ như sổ cái phân tán, đồng thuận,
> tài sản số, rủi ro và tính minh bạch giao dịch. System prompt định hướng cách
> model chọn giọng văn, độ sâu nội dung và loại ví dụ. Vì vậy cùng một câu hỏi
> nhưng đối tượng nghe khác nhau sẽ tạo ra câu trả lời rất khác nhau.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> `tiktoken` đếm được 107
> token, còn ước lượng `số từ / 0.75` là 120 token, chênh khoảng 10,8% theo
> hướng ước lượng cao hơn trong ví dụ này. Tiếng Việt thường tốn nhiều token vì
> có dấu, nhiều âm tiết tách bằng khoảng trắng và tokenizer có thể phải chia một
> từ hoặc cụm có dấu thành nhiều mảnh nhỏ hơn so với các từ tiếng Anh phổ biến.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi phản hồi dài hoặc người dùng cần cảm giác hệ
> thống đang trả lời ngay, ví dụ chatbot, trợ lý viết nội dung, giải thích bài
> học hoặc sinh code nhiều dòng. Nó giảm thời gian chờ cảm nhận được vì người
> dùng thấy từng phần kết quả xuất hiện dần. Non-streaming phù hợp hơn khi kết
> quả ngắn, cần xử lý nguyên khối trước khi hiển thị, cần parse JSON chính xác,
> hoặc backend chỉ cần nhận toàn bộ câu trả lời để lưu log
### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm áp lực lên API bằng cách chờ lâu hơn sau mỗi lần
> thất bại, nhờ đó server có thời gian hồi phục và client không tiếp tục dồn
> request quá nhanh. Nếu hàng nghìn client đều retry với delay cố định giống
> nhau, chúng có thể gửi lại request cùng lúc theo từng đợt, làm hệ thống tiếp
> tục quá tải. Backoff, đặc biệt khi có thêm jitter, giúp phân tán thời điểm
> retry và tăng khả năng thành công.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona em chọn là trợ giảng AI thân thiện cho người mới học lập trình.
> System prompt: "Bạn là trợ giảng AI thân thiện, giải thích bằng tiếng Việt rõ
> ràng, ngắn gọn, ưu tiên ví dụ thực tế và chỉ ra lỗi phổ biến khi người học
> viết code. Khi câu hỏi mơ hồ, hãy hỏi lại một câu ngắn trước khi trả lời." Em chỉ định "bằng tiếng Việt" để phù hợp với người học trong lớp, và dùng "ngắn
> gọn" để câu trả lời dễ theo dõi trong môi trường CLI.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ giữ 3 lượt gần nhất, nên trợ lý dễ quên thông
> tin quan trọng nếu cuộc trò chuyện kéo dài. Một cải thiện cụ thể là thêm cơ
> chế tóm tắt hội thoại (khi history vượt quá 6 message, gọi model tạo một bản
> tóm tắt ngắn các thông tin quan trọng rồi đưa bản tóm tắt đó vào system hoặc
> developer message ở các lượt sau). Cách này giữ được ngữ cảnh dài hơn mà vẫn
> kiểm soát số token.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
