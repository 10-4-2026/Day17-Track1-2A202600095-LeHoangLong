# Day 17 Submission
**Student:** Lê Hoàng Long 2A202600095
**Date:** 24-04-2026
**Product idea:** ghi âm bài giảng trên lớp, tìm kiếm những nội dung được nhấn mạnh nhiều lần trong bài giảng

---

### 1. MVP Boundary Sheet

**Riskest Assumption:**
> 

**In-Scope**(tối đa 3):
- [][Tính năng 1 -- ghi âm, chuyển sang văn bản] -- test giả định: [người dùng có săn bản ký âm chính xác tầm 80%]
- [][Tính năng 2 -- phát hiện các cụm từ, ý lặp lại] -- test giả định: [ các phương pháp đếm từ đủ để để phản ánh ý quan trọng trong bài]
- [][Tính năng 3 -- giao diện hiển thị đơn giản các nội dung đáng chú ý] -- test giả định: [ người dùng xem và thấy nội dung đúng thứ họ cần]


**Out-of-Scope:**:
- [Tính năng A : nhận diện ý quan trọng bằng AI ngữ nghĩa] -- lý do bỏ: [chi phí lớn]
- [Tính năng B: xử lý thời gian thực] -- lý do bỏ: [ tăng chi phí xử lý luồng]

**Non-Goals:**
- [Ranh giới đỏ 1] : không cố thay thể việc ghi chú và học
- [Ranh giới đỏ 2] : không đảm bảo độ chính xác 100%

---

## 2.PRD Skeleton

### Problem Statement
> [1 câu: Ai, đang gặp vấn đề gì, hậu quả kinh tế/ vận hành là gì]
> Sinh viên ghi âm bài giảng, không thể nhanh chóng tìm lại ý chính của buổi học, phải nghe lại toàn bộ, giảm hiệu quả

### Target User
> [Chân dung cụ thể - nối từ Customer Segment Card]
> sinh viên thường xuyên phải ghi âm buổi học các môn nhiều lý thuyết, cần ôn tập nhanh nhung không có thời gian nghe lại toàn bộ


### User Stories
**Story 1:**
> As a [...], I want [...], so that [...]
> as a student, I want to record and convert the lecture into document, so that I can read

**Story 2:**
> As a [...], I want [...], so that [...]
> as a student, I want to have a quick looking at stressed phrase, paragraph, so that I could quickly catch the main point of the lecture

### AI-Specific

**Model Selection:**
- Model: [tên model] ASR: OpenAI Whisper, text processing GPT-4o mini. Nếu không dùng dịch vụ trực tuyến có thể dùng Llama 3 8B trên ollama, gguf, thu hoạch từ khóa với KeyBERT 
- Lý do chọn: [giải thích] Whisper mạnh với mô hình đa ngôn ngữ và môi trường ồn ào. GPT-4o mini đủ tốt để tóm tắ văn bản và phát hiện mẫu. Kiến trúc đơn giản audio -> text -> insight
- Trade-offs chấp nhận: [...] độ chính xác thấp, ý chính quan trọng có thể không hoàn toàn chính xác, độ trễ từ vài giây đến vài phút
- Trade-offs không chấp nhận: [...] Bản ký âm sai hoặc không hiểu được, mục đánh dấu lệch nội dung giảng, thời gian xử lý quá lâu

**Data Rrequirement:**
- Nguồn: [...] tập tin âm thanh 
- Owner: [...] dữ liệu cá nhân
- Update frequency: [...] cập nhật khi có bản ký âm mới

**Fallback UX:**
- Chiến lược: [Expectation Management/ Human-in-the-loop/Graceful Handover] AI chỉ gợi ý, không khăng định 100%
- Trigger: [Khi nào AI bị coi là không đủ tự tin] độ tự tin bản ký âm thấp, nội dung lặp lại không rõ ràng, âm thanh bị nhiễu, nhiều người nói chồng chéo lên nhau.
- Hành động: [Hệ thống làm gì cụ thể] hiển thị cảnh báo, chuyến sang hiện bản ký âm, ghi chú từ khóa, không suy luận sâu
- User options: [User có thể làm gì tiếp theo]. Người dùng nghe lại tập tinh âm thanh, sửa bản ký âm thủ công, đánh dấu thủ công, đối bản thu âm khác tốt hơn

### Sucess Metrics
- Primary metrics: [...] % người dùng bấm vào , sao chép mục được đánh dấu. thời gian từ khi mở bản tóm tắt cho đến khi bấm vào mục được đánh dấu đầu tiên, nhận đánh giá từ khách hàng
- Ngưỡng thành công: [...] lớn hơn 60% người dùng tìm được nội dung bị đánh dấu phù hợp, thời gian để xử lý nhỏ hơn 30 giây
- Timeframe đo lường: [...] tuần 3-4 thử nghiệm người dung thật, tháng 1 tập trung cho tóm tắt hoặc ghi chú

### Dependencies & Constraints
- [...]
- ASR: OpenAI whipsper , âm thanh thành văn bản
- LLM gpt-4o mini : phát hiện ý nhấn mạnh
- ffmpeg xử lý file
- backend gpt api
- frontend/ui: streamlit/ web app
- Constraints: gpt cần kết nối ổn định
- GPT tính phí theo token
- tập tin âm thanh dài cần chunking và hạn chế ngữ cảnh hoặc chia nội dung theo từng khối , xử lý sau đó ghép các kết quả với nhau
- âm thanh gửi qua gpt nên cần thông báo cho khách hàng
- whisper chịu hạn chế về chất lượng âm thanh đầu vào
- gpt chịu sự hạn chế về chi phí vận hành và độ trễ khi xử lý  

---

## Hypothesis Table

### Hypothesis 1 (Cho tính năng In-Scope #1)
> Chúng tôi tin rằng [...] sẽ giúp [...] đạt được [...]
> Chúng tôi sẽ biết mình đúng khi thấy [metric] đạt [ngưỡng] trong [thời gian].

Rickiest assumption: [...] Transcript đủ chính xác để người dùng hiểu (đặc biệt với tiếng Việt + môi trường lớp học ồn)
Cách test cheapest: [...] → Lấy 5–10 file ghi âm thật (không cần app hoàn chỉnh)
→ chạy Whisper → đưa transcript dạng text đơn giản (Google Doc / Notion)
→ hỏi user: “Bạn có dùng cái này để ôn không?”

### Hypothesis 2 (nếu có)
> [...]
> Chúng tôi tin rằng việc dùng GPT-4o mini để phát hiện các ý/cụm từ được nhấn mạnh sẽ giúp sinh viên xác định nội dung quan trọng nhanh hơn, giảm thời gian ôn tập.

> Riskiest assumption:
→ “Ý được lặp lại nhiều” thực sự tương đương với “ý quan trọng” trong bài giảng

Cách test cheapest: dùng word frequency / n-gram đơn giản highlight
→ so sánh với highlight do user tự chọn
→ hỏi: “Cái nào đúng hơn?”
---

## 4. PMF Scorecard

** Aha Moment:**
> [Mô tả hành vi cụ thể - không phải cảm giác]
> người dùng mở đoạn ký âm, bấm vào mục đánh dấu, ngay lập tức được máy chuyển đến nội dung cần đọc hoặc đoạn âm thanh cần nghe

**Actionable Metric:**
> [Tên metric + cách đo] = số hành vi đọc kỹ hơn hoặc thời gian đọc kỹ hơn / số lần bấm mục đánh dấu

** PMF Method:**
>[Sean Ellis/ Retention Curve/ Aha Moment tracking] : Retention Curve/ Aha Moment tracking
> Ngưỡng thành công: [...] ≥ 50% user đạt Aha Moment trong lần dùng đầu
≥ 30% retention (D7)
≥ 20% retention (D30)

**Vanity Metrics tôi sẽ không dùng:**
- [...]
- Tổng số phút audio đã xử lý
- Số lượng transcript tạo ra
- Số highlight được generate (AI output ≠ value)
- Số user đăng ký nhưng không dùng lại


## 5. AI Critique log

**Điểm AI chỉ ra:**
1. [Issue 1] -- action: accept/ reject/partial - lý do: [...] “Ý lặp lại ≠ ý quan trọng”
-- action: accept
-- lý do:

Giảng viên có thể lặp lại để giải thích, không phải để nhấn mạnh
Một số ý quan trọng chỉ nói 1 lần nhưng rất critical
→ Nếu chỉ dùng frequency → sản phẩm dễ “trông thông minh nhưng sai bản chất”
2. [Issue 2] -- action: accept/ reject/partial - lý do: [...] Chất lượng transcript là single point of failure (ASR từ OpenAI Whisper)
-- action: accept
-- lý do:

Audio lớp học: ồn, nhiều người nói → ASR sai
Sai từ khóa → GPT (GPT-4o mini) suy luận sai dây chuyền
→ Đây là rủi ro kỹ thuật lớn nhất, không phải LLM
3. [Issue 3] -- action: accept/ reject/partial - lý do: [...] User có thể không thay đổi hành vi (vẫn thích nghe lại)
-- action: partial
-- lý do:

Một số người học bằng nghe (auditory learners)
Nhưng đa số vẫn cần scan nhanh trước kỳ thi
→ Không nên assume replace audio, mà là augment


4. [Issue 4] -- action: accept/ reject/partial - lý do: [...]
Latency + cost (GPT) có thể phá trải nghiệm
-- action: accept
-- lý do:

Nếu chờ quá lâu → user bỏ
Nếu cost cao → không scale được
→ cần fallback heuristic + chunking

**Thay đổi lớn nhất giữa Version A và Version B: **
Version A:

Giả định: “AI sẽ tự động tìm ra ý quan trọng”
Pipeline: đơn giản (ASR → GPT → highlight)
Mindset: AI-centric

Version B:

Nhận ra: “frequency ≠ importance” + “ASR là bottleneck”
Pipeline: hybrid (heuristic + AI + fallback)
Thêm:
confidence-based downgrade
user-in-the-loop (edit / highlight)
focus vào time-to-insight, không phải “AI thông minh”

---

### 6. Self-assessment

Mắt xích nào trong [MVP Boundary PRD Hypothesis PMF] bạn đang yếu nhất/
> Trả lời thật
> Mắt xích yếu nhất

Hypothesis (đặc biệt là giả định “ý lặp lại = ý quan trọng”)

Trả lời thật:

Đây là giả định nguy hiểm nhất vì nó quyết định trực tiếp value của sản phẩm
Nếu sai → toàn bộ pipeline (dù dùng OpenAI Whisper hay GPT-4o mini) vẫn tạo ra output không hữu ích
Hiện tại mới validate ở mức “logic hợp lý”, chưa có evidence từ user behavior

Cụ thể:  đang build solution trước khi chứng minh problem-solution fit

Open question bạn muốn giải đáp tiếp
1.[...] “Ý quan trọng” thực sự được signal bằng gì trong bài giảng?
Lặp lại?
Giọng nhấn mạnh? (prosody)
Từ khóa như “quan trọng”, “thi sẽ ra”?
Slide / context xung quanh?

→ Đây là câu hỏi cốt lõi, quyết định  có cần GPT hay không
2.[...] Highlight = intermediate step
Có thể user chỉ cần:
→ “Top 10 ý cần học cho kỳ thi”

→ Nếu sai abstraction → sản phẩm sẽ không được dùng