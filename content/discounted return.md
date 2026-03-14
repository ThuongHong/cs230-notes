
**Discounted return (Tổng phần thưởng chiết khấu)** là tổng toàn bộ phần thưởng mà AI nhận được từ hiện tại đến tương lai, trong đó phần thưởng ở càng xa thì giá trị càng bị giảm (nhân với $\gamma^t$).

- **Công thức:** $R = r_0 + \gamma r_1 + \gamma^2 r_2 + \dots$
    
- **Ý nghĩa:** Đây là **mục tiêu cuối cùng** mà AI tìm cách tối đa hóa. Thay vì chỉ nhìn vào phần thưởng ngay trước mắt ($r_0$), giá trị này giúp AI đánh giá được lợi ích tổng thể của _cả một chuỗi hành động dài hạn_.

---
![[Pasted image 20260228173649.png]]

Hệ số chiết khấu (**discount factor**, ký hiệu $\gamma$) trong Học tăng cường dùng để xác định **mức độ ưu tiên giữa phần thưởng hiện tại và phần thưởng trong tương lai**.

Mục đích chính của nó là:

- **Khuyến khích nhận thưởng sớm:** Việc nhân với hệ số $\gamma < 1$ làm cho giá trị của các phần thưởng ở tương lai xa bị giảm đi. Điều này giúp tác tử (agent) có xu hướng muốn đạt được mục tiêu và nhận thưởng càng sớm càng tốt.
    
- **Tránh tổng phần thưởng bị vô hạn:** Trong các bài toán không có điểm kết thúc rõ ràng, việc chiết khấu giúp công thức tính tổng phần thưởng (Discounted return) hội tụ về một con số hữu hạn để dễ tính toán.
    

**Tóm lại:**

- $\gamma$ gần **0**: Agent trở nên "thiển cận", chỉ quan tâm đến phần thưởng ngay trước mắt.
    
- $\gamma$ gần **1**: Agent có tầm nhìn xa, chịu khó đi đường vòng để đạt phần thưởng lớn hơn ở tương lai (như trạng thái S5 được +10 điểm trong hình).

---
# Ví dụ

**1. Ví dụ toán học (Dựa trên hình ảnh của bạn)**

Giả sử AI xuất phát từ S2 và đi sang phải đến S5 (đường đi: `S2 -> S3 -> S4 -> S5`). Hệ số $\gamma = 0.9$.

- Phần thưởng từng bước: $r_0=0$ (S2), $r_1=0$ (S3), $r_2=1$ (S4), $r_3=10$ (S5).
    
- **Discounted return ($R$):**
    
    $R = 0 + (0.9 \times 0) + (0.9^2 \times 1) + (0.9^3 \times 10)$
    
    $R = 0 + 0 + 0.81 + 7.29 = 8.1$
    
    👉 **Ý nghĩa:** Ở bước S2 và S3 dù không có phần thưởng, nhưng nhờ $R = 8.1$, AI vẫn nhận ra "đi về hướng này rất có giá trị" thay vì đứng im.
    

**2. Ví dụ thực tế (Chơi cờ vua)**

- **Lựa chọn A (Ăn tốt ngay lập tức):** Bạn ăn được con tốt của đối thủ ở nước đi này ($r_0 = +1$), nhưng sau đó mất lợi thế ($r_1, r_2 = 0$).
    
- **Lựa chọn B (Dàn trận chiếu tướng):** Bạn không ăn tốt, chịu hy sinh một chút ($r_0 = -1$), nhưng 3 nước sau bạn chiếu bí đối thủ ($r_3 = +100$).
    
    👉 Nếu hệ số chiết khấu $\gamma$ **cao** (tầm nhìn xa), $R$ của Lựa chọn B sẽ lớn hơn rất nhiều, AI sẽ bỏ qua con tốt để đi đến chiến thắng cuối cùng.
    
    
