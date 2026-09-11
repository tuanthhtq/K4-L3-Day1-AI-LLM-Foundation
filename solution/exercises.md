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
> temperature càng cao thì câu trả lời càng có biến đổi nhiều hơn, temperature càng cao thì câu trả lời sẽ có cách diễn đạt đa dạng hơn

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> em sẽ dùng temperature 0.3 vì câu trả lời cho khách cần phải nhất quán, rõ ràng và bám sát thông tin đã được cung cấp

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> ước tính gpt-4o đắt dơn 16.67 lần so với gpt-4o-mini chochi phí token đầu ra. gpt-4o nên được dùng khi phân tích tài liệu phức tạp, liên kết nhiều thông tin và cần câu trả lời chính xác cao. gpt-4o-mini nên dùng khi làm việc với các tác vụ phổ thông, các câu hỏi thường gặp hoặc trả lời các câu hỏi có tài liệu rõ ràng 

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> vói persona giáo viên tiểu học, câu trả lời sẽ thường có những câu ngắn, từ ngữ dễ hiểu và đơn giản. với persona chuyên gia, phản hồi sẽ chi tiết hơn, dùng các thuật ngữ chuyên ngành. system prompt định hướng vai trò, cách trình bày và mức độ chuyên sâu

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> ví dụ với đoạn văn sau "Trí tuệ nhân tạo đang trở thành công cụ hữu ích trong học tập và công việc. Sinh viên có thể dùng trợ lý ảo để tìm ý tưởng, giải thích khái niệm khó và luyện viết mỗi ngày. Tuy nhiên, người dùng cần kiểm tra thông tin trước khi áp dụng vì mô hình vẫn có thể trả lời sai. Khi đặt câu hỏi, hãy nêu rõ mục tiêu, cung cấp bối cảnh và yêu cầu định dạng kết quả. Cách làm này giúp câu trả lời phù hợp hơn, tiết kiệm thời gian và hỗ trợ việc học.". công thức tính token ở part 1 sẽ cho ra  khoản 133 token, đối với count_tokens sẽ cho ra 119 token -> chênh lệch khoản 10.75%. tiếng Việt thường tốn token hơn so với tiếng Anh vì bộ từ vựng của tokenizer thiên về tiếng Anh, ký tự có dấu của tiếng Việt tốn nhiều byte hơn và cách tokenizer chia âm tiết cũng khác nhau

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> streaming phù hợp với các app chat bot vì người dùng  có thể đọc được câu trả lời ngay khi phần đâu được sinh ra do đó người dùng cảm thấy thời gian chờ câu trả lời ngắn. non-streaming phù hợp khi dùng các tác vụ cần đủ kết quả để kiểm tra, phân tích json

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> lợi ích là giúp giản áp lực lên api và cho hệ thống có thời gian phục hồi thay vì phải đón các đợt retry hàng loạt cùng lúc. nếu retry với delay cố định giống nhau thì hệ thống sẽ nhận tất cả request retry cùng 1 lúc, gây ra quá tải

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> em chọn personal trợ giản cho người học ai với system prompt "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt.". cụm "trợ giảng thân thiện" định hướng câu trả lời dễ tiếp cận, dễ hiểu, phù hợp với người mới học. cụm "trả lời ngắn gọn" yêu cầu câu trả lời tập trung vào ý hỏi của người hỏi

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> hạn chế hiện tại là đối với đoạn hội thoại dài thì chatbot có thể quên ngữ cảnh từ đầu. cải thiện có thể là thay vù truyền toàn bộ lịch sử chat thì sẽ tóm tắt chúng lại rồi mới truyền cho ai

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
