# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Ở temperature 0.0, câu trả lời lặp lại gần như giống hệt nhau nếu gọi nhiều lần. Lên mức 0.5 và 1.0, mô hình cung cấp các thông tin đa dạng, tự nhiên hơn. Tuy nhiên, khi đẩy lên mức 1.5, có tạo ra những thông tin không có thật.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt ở mức thấp (0.1 - 0.2) vì Chatbot hỗ trợ khách hàng cần sự chính xác, nhất quán và tuân thủ chặt chẽ theo chính sách/kịch bản công ty.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Hiện tại, giá output của GPT-4o đắt gấp khoảng 25 lần so với GPT-4o-mini. Khi hệ thống cần xử lý các tác vụ suy luận phức tạp, phân tích mã nguồn (coding), hoặc đọc hiểu các tài liệu pháp lý đòi hỏi độ chính xác và tư duy logic cao, sử dụng GPT-4o. Trường hợp các tính năng có lượng truy cập lớn nhưng tác vụ đơn giản như tóm tắt bài báo, dịch thuật cơ bản, ... thì sử dụng model mini.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Phản hồi của "giáo viên tiểu học" ngắn gọn, dùng từ ngữ gần gũi hơn, sử dụng cụm "cuốn sổ tay dùng chung" để giải thích. Ngược lại, "chuyên gia tài chính" đưa ra phản hồi dài hơn, cấu trúc phức tạp với nhiều thuật ngữ chuyên ngành như "sổ cái phân tán" (distributed ledger), "mã hóa thuật toán", "node", "phi tập trung". Qua đó, có thể thấy System prompt hoạt động như một "bộ lọc hành vi", thiết lập bối cảnh cốt lõi buộc mô hình phải căn chỉnh lại toàn bộ vốn từ vựng, văn phong và mức độ chi tiết để nhập vai phù hợp.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Số lượng token tiếng Việt thường cao hơn ước lượng khoảng 150-250%. Nguyên nhân là do các bộ Tokenizer của OpenAI được tối ưu hóa chủ yếu trên khối lượng dữ liệu tiếng Anh khổng lồ. Một từ tiếng Anh thường là 1 token, nhưng một từ tiếng Việt (do hệ thống dấu thanh và cách phân tách âm tiết) thường bị chia cắt vụn thành 2, 3 hoặc thậm chí nhiều token nhỏ hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt quan trọng trong các ứng dụng tương tác thời gian thực với con người như chatbot, giúp giảm thiểu "độ trễ cảm nhận" (perceived latency) bằng cách cho phép người dùng đọc ngay từng từ khi được tạo ra, thay vì phải nhìn màn hình chờ đợi tới khi hoàn thành câu trả lời 15-20 giây. Non-streaming lại là lựa chọn tối ưu cho việc trả lời dữ liệu có cấu trúc (như JSON) hoặc các tác vụ tự động hóa mà chỉ cần kết quả nguyên vẹn cuối cùng để máy tính xử lý bước tiếp theo.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm tải dần dần cho server bằng cách giãn cách thời gian giữa các lần thử lại ngày càng xa nhau. Nếu hàng nghìn client cùng sử dụng một delay cố định 1 giây, chúng sẽ đồng loạt tái gửi request vào cùng một tíc tắc, tạo thành các đợt tấn công DDoS cục bộ khiến server đã quá tải lại càng dễ bị sập hẳn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: Chuyên viên tóm tắt tin tức công nghệ.
System Prompt: "Bạn là một biên tập viên công nghệ. Hãy tóm tắt bài viết người dùng cung cấp thành đúng 3 gạch đầu dòng ngắn gọn. Bắt buộc sử dụng tiếng Việt. Không thêm bất kỳ thông tin bình luận hay suy diễn cá nhân nào ngoài lề."
Giải thích: (1) Việc giới hạn "đúng 3 gạch đầu dòng ngắn gọn" giúp chuẩn hóa độ dài đầu ra. (2) Cụm từ "Không thêm bất kỳ thông tin... ngoài lề" là chốt chặn quan trọng nhằm hạn chế việc mô hình lan man vào việc diễn giải, suy diễn và đề cập sang các chủ đề công nghệ khác không có trong văn bản gốc.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất: History còn ngắn, với cuộc hội thoiaj dài có thể sử dụng tóm tắt lại nội dung cuộc trò chuyện trước đó thay vì chỉ ghi lại 3 lượt gần nhất.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
