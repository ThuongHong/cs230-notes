---
tags:
  - dl
  - stanford
  - cs230
class: cs230
date: 2026-02-09T23:15:00
---
https://youtu.be/MGqQuQEUXhk?si=WVSISEf4vhcQiHxE

# Full Cycle of a DL project

AI = Code + Data

Steps for a DL project:
1. Specify problem
2. Get data
3. Design model
4. Train model
5. Deploy model
6. Monitor/Maintain model
Step 2, 3, 4 lặp lại nhiều lần cho đến khi đạt kết quả tốt

*Vocab*:
- gravitate (to) sth: to be attracted to or move toward sth

Thầy Andrew nói rằng sẽ ưu tiên những phương pháp thu thập dữ liệu nhanh (trong khoảng 1-2 ngày), ngay cả khi chất lượng bộ dữ liệu thấp hơn. Vì ta sẽ không biết được vấn đề gì xảy ra với data của mình, càng nhanh có được data -> phát triển model nhanh -> phát hiện vấn đề -> quay lại khắc phục 

Cần cân bằng thời gian cho 3 step trên. Ví dụ: nếu thời gian train model chỉ 1-2 ngày thì việc design model, thu thập data cũng chỉ nên tầm đó.

Việc hiểu rõ kiến trúc mô hình, các tham số.... sẽ giúp ta bớt *thử-sai* (tiết kiệm thời gian hơn).

Nếu 1 problem mà trước đó chưa bao giờ làm qua, hoặc không có kinh nghiệm, thì ta nên chú trọng vào tốc độ thực hiện 3 step trên.

Thay vì tập trung vào prompt engineering, thử nhiều hơn.

Không phải cứ thu thập nhiều data là tốt, data chất lượng mới tốt.  

How important is it that the data you collect is similar to the distribution of things you want to work on? (Ý là mấy cái data trong train set mình thu thập khác với target mà mình muốn thì sao? Nó có ảnh hưởng gì nhiều không)
>Thường thì với mạng NN lớn sẽ không ảnh hưởng lắm, thậm chí còn có thể có ích. Việc có thêm các data không liên quan có thể giúp NN học được nhiều hơn... 
>Tuy nhiên trước đây, người ta thường ám ảnh với việc test set phải cùng phân phối với train set; hiện giờ thì thoải mái hơn. 
>Tất nhiên điều trên chỉ đúng với NN đủ lớn.


**Sense of a deployment**.

Bối cảnh: Ta đang cần xây dựng hệ thống face verification trong camera. 
>Việc cho NN liên tục thực hiện task sẽ rất tốn kém. Thay vì thế, người ta thường sử dụng 1 hệ thống VAD (visual activity detection), cái này chạy nhanh và đỡ tốn kém hơn. VAD sẽ quyết định khi nào NN cần làm việc. Ta xem xét 2 option cài đặt VAD sau:
>- Option 1 (non ML): Kiểm tra xem pixel có thay đổi nhiều hơn 1 ngưỡng nào đó 
>- Option 2 (NN-based): Huấn luyện NN nhỏ để nhận biết frame nào có người, frame nào không có người...
>Thật ra từ ban đầu, ta khó trả lời rằng option nào tốt hơn (về mặt tốc độ, hiệu quả...), nó phụ thuộc vào nhiều yếu tố. Nói chung là phải thử mới biết được.

Khi nào ta biết hệ thống của ta đủ tốt?
>Khó nói. Thường thì tốt hơn hoặc bằng con người là đủ.


**Monitor/Maintain model**

Nhiều khi hệ thống của ta hoạt động tốt trên test set, ra ngoài thực tế lại ngu, có thể do input khó, do data shift, concept shift... 
Ví dụ: hệ thống hỗ trợ tự lái hoạt động tốt ở Mỹ, nhưng về Việt Nam thì chưa chắc...

Liệt kê các vấn đề có thể xảy ra khi deploy, xây dựng dashboard hoặc metric để theo dõi chúng và giải quyết.