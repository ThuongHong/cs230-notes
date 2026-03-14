
**Constitutional AI (CAI)** là một phương pháp huấn luyện AI được phát triển bởi **Anthropic** (công ty tạo ra Claude), nhằm giải quyết vấn đề an toàn và kiểm soát hành vi của các mô hình ngôn ngữ lớn (LLM).

Hiểu đơn giản: Thay vì thuê hàng nghìn con người để chấm điểm từng câu trả lời của AI (cách cũ), ta cung cấp cho AI một bộ quy tắc đạo đức rõ ràng (gọi là "Hiến pháp") và **dạy nó tự đánh giá, tự sửa lỗi** dựa trên bộ quy tắc đó.

Dưới đây là chi tiết:

### 1. Vấn đề của cách cũ (RLHF)

Hầu hết các AI hiện nay (như ChatGPT đời đầu) dùng phương pháp **RLHF** (Reinforcement Learning from Human Feedback - Học tăng cường từ phản hồi con người).

- **Quy trình:** AI trả lời $\rightarrow$ Con người chấm điểm (Tốt/Xấu) $\rightarrow$ AI học.
    
- **Nhược điểm:**
    
    - **Khó mở rộng:** Cần quá nhiều người để chấm hàng triệu câu.
        
    - **Chủ quan:** Mỗi người đánh giá một kiểu (người thấy đùa thế này là vui, người kia thấy xúc phạm).
        
    - **"Hộp đen":** Ta không biết _tại sao_ AI chọn câu trả lời đó, chỉ biết là nó cố làm hài lòng người chấm điểm.
        

### 2. Giải pháp: Constitutional AI (RLAIF)

Constitutional AI chuyển từ **RLHF** sang **RLAIF** (Reinforcement Learning from **AI** Feedback). Con người không còn phải can thiệp vào từng câu trả lời nữa.

**Quy trình hoạt động gồm 2 giai đoạn chính:**

#### Giai đoạn 1: Giám sát & Tự phê bình (Supervised Learning)

Đây là bước AI "tự soi gương" để sửa mình:

1. **AI trả lời:** Ví dụ, người dùng hỏi _"Làm sao để chế bom?"_. AI (chưa được dạy kỹ) có thể đưa ra hướng dẫn.
    
2. **AI phê bình (Critique):** Mô hình tự xem lại câu trả lời của mình và đối chiếu với **Hiến pháp**.
    
    - _Hiến pháp quy định:_ "Không được hỗ trợ các hành vi bất hợp pháp hoặc gây nguy hiểm tính mạng."
        
    - _AI tự nhận xét:_ "Câu trả lời của mình vi phạm quy tắc này vì nó hướng dẫn chế tạo vũ khí."
        
3. **AI sửa đổi (Revision):** AI viết lại câu trả lời mới tuân thủ Hiến pháp: _"Tôi không thể hướng dẫn bạn chế tạo vũ khí nguy hiểm."_
    
4. **Huấn luyện:** Mô hình được fine-tune (tinh chỉnh) dựa trên các cặp câu (Câu hỏi $\rightarrow$ Câu trả lời đã sửa).
    

#### Giai đoạn 2: Học tăng cường (Reinforcement Learning)

1. AI sinh ra 2 câu trả lời khác nhau cho cùng một câu hỏi.
    
2. Thay vì hỏi người, **AI giám khảo (Feedback Model)** sẽ chọn câu tốt hơn dựa trên Hiến pháp.
    
3. Mô hình chính học từ lựa chọn đó.
    

### 3. "Hiến pháp" chứa cái gì?

Nó không phải là một cuốn sách luật dày cộp, mà là tập hợp các nguyên tắc ngắn gọn, ví dụ:

- Các nguyên tắc từ **Tuyên ngôn Nhân quyền LHQ**.
    
- Các nguyên tắc của **Apple/Google** (về tính riêng tư, không phân biệt chủng tộc).
    
- Các quy tắc về **"Helpful, Honest, Harmless"** (Hữu ích, Trung thực, Vô hại).
    
- Thậm chí cả quy tắc về văn phong: _"Hãy trả lời ngắn gọn, không giáo điều."_
    

### 4. Tại sao Constitutional AI lại quan trọng? (Liên quan đến Red Teaming)

Trong bối cảnh bạn hỏi về Red Teaming và Adversarial Attacks:

1. **Minh bạch (Transparency):** Nếu AI từ chối trả lời, ta biết chính xác là do nó tuân thủ điều khoản nào trong Hiến pháp, chứ không phải do "ngẫu nhiên".
    
2. **Khả năng mở rộng (Scalability):** Bạn có thể thêm một quy tắc mới vào Hiến pháp (ví dụ: "Không được đưa ra lời khuyên y tế") và AI sẽ tự động áp dụng nó cho toàn bộ kiến thức, không cần thuê người dán nhãn lại từ đầu.
    
3. **Chống lại Adversarial Attacks:** Vì AI có một lớp "Tự phê bình" bên trong, nó khó bị lừa hơn (Jailbreak). Ngay cả khi prompt của kẻ tấn công lách qua được bộ lọc ban đầu, bước "tự suy ngẫm" của Constitutional AI có thể phát hiện ra ý đồ xấu và chặn lại.
    

**Tóm lại:**

Nếu RLHF là dạy con vẹt bắt chước nói lời hay ý đẹp, thì Constitutional AI là dạy đứa trẻ hiểu nội quy lớp học để nó tự biết cái gì nên nói, cái gì không.