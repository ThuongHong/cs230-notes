![[Pasted image 20260214155118.png]]

Hình này giải thích **mẹo kỹ thuật** (heuristic) quan trọng để giúp Generator học được, đặc biệt là giai đoạn đầu.

Cốt lõi là thay đổi hàm mục tiêu để **khắc phục lỗi triệt tiêu đạo hàm (vanishing gradient)**:

1. **Đường màu cam (Saturating cost - Lý thuyết gốc):**
    
    - Công thức: $J^{(G)} = \frac{1}{m} \sum \log(1 - D(G(z)))$.
        
    - Mục tiêu: _Giảm thiểu_ khả năng bị phát hiện là giả.
        
    - **Vấn đề:** Khi mới bắt đầu train, Generator tạo ảnh rất tệ nên Discriminator dễ dàng đoán ra ($D(G(z)) \approx 0$).
        
    - **Nhìn đồ thị:** Tại điểm 0, đường cam **nằm ngang (flat)**. Điều này nghĩa là đạo hàm (gradient) gần bằng 0 $\rightarrow$ Generator không học được gì. Đây gọi là "bão hòa" (saturation).
        
2. **Đường màu xanh (Non-saturating cost - Thực tế sử dụng):**
    
    - Công thức: $J^{(G)} = -\frac{1}{m} \sum \log(D(G(z)))$.
        
    - Mục tiêu: _Tối đa hóa_ khả năng lừa được Discriminator (làm cho $D$ nghĩ ảnh là thật).
        
    - **Ưu điểm:** Tại điểm 0, đường xanh **rất dốc**. Điều này tạo ra đạo hàm (gradient) lớn $\rightarrow$ Giúp Generator học rất nhanh và mạnh mẽ ngay từ đầu.
        

**Tóm lại:** Thay vì cố gắng "không bị bắt" (đường cam), Generator nên cố gắng "chiến thắng" (đường xanh) để có tín hiệu học tốt hơn.