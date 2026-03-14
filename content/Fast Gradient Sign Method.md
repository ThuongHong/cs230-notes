
**Fast Gradient Sign Method (FGSM)** là thuật toán kinh điển do Ian Goodfellow giới thiệu năm 2014. Đây là hiện thân rõ nhất của việc lợi dụng **"tính tuyến tính"** để tấn công mạng Neural.

### 1. Ý tưởng cốt lõi: "Train ngược"

- **Training bình thường:** Bạn giữ nguyên _Input_, chỉnh sửa _Weights_ để **giảm** lỗi (Loss).
    
    - Mục tiêu: $\min Loss$.
        
- **Tấn công FGSM:** Bạn giữ nguyên _Weights_, chỉnh sửa _Input_ để **tăng** lỗi lên mức cao nhất.
    
    - Mục tiêu: $\max Loss$.
        

### 2. Công thức

Công thức tạo ra ảnh đối kháng ($x_{adv}$) từ ảnh gốc ($x$):

$$x_{adv} = x + \epsilon \cdot \text{sign}(\nabla_x J(\theta, x, y))$$

**Giải thích từng thành phần:**

1. **$x$:** Ảnh gốc (ví dụ: ảnh con gấu trúc).
    
2. **$y$:** Nhãn đúng (label: "Gấu trúc").
    
3. **$J(\theta, x, y)$:** Hàm mất mát (Loss function) dùng để đo sai số dự đoán.
    
4. **$\nabla_x$ (Gradient theo Input):** Đây là phần quan trọng nhất. Nó cho biết: _"Nếu muốn lỗi tăng lên nhanh nhất, từng pixel phải thay đổi theo hướng nào?"_
    
5. **$\text{sign}()$ (Hàm dấu):**
    
    - Nếu gradient dương $\to$ Lấy +1.
        
    - Nếu gradient âm $\to$ Lấy -1.
        
    - _Đây chính là bước "tối ưu hóa tuyến tính"._
        
6. **$\epsilon$ (Epsilon):** Một số rất nhỏ (ví dụ 0.007). Đây là lượng nhiễu tối đa cho phép để mắt thường không nhận ra.
    

### 3. Tại sao lại dùng hàm `sign()`? (Liên hệ với Tính Tuyến tính)

Tại sao không cộng trực tiếp Gradient mà phải lấy dấu (`sign`)?

Hãy nhớ lại công thức thay đổi output: $\Delta y = w^T \eta = \sum w_i \eta_i$.

Để $\Delta y$ lớn nhất với một lượng nhiễu $\eta$ bị giới hạn (không được quá $\epsilon$), cách tốt nhất là **luôn di chuyển tối đa** theo hướng của trọng số $w$:

- Nếu $w_i$ dương, ta cộng thêm $\epsilon$ (làm pixel sáng lên chút xíu).
    
- Nếu $w_i$ âm, ta trừ đi $\epsilon$ (làm pixel tối đi chút xíu).
    

Hàm `sign()` đảm bảo ta luôn tận dụng triệt để "ngân sách" $\epsilon$ tại **mọi pixel**.

### 4. Ví dụ trực quan

1. **Bước 1:** Mạng NN nhìn ảnh con Gấu trúc.
    
2. **Bước 2:** Tính toán Gradient. Mạng "nói" rằng: _"Nếu pixel số 1 sáng hơn, pixel số 2 tối đi,... thì tôi sẽ dễ nhầm con này là con Vượn hơn."_
    
3. **Bước 3 (FGSM):** Ta thực hiện chính xác điều đó. Cộng một lớp nhiễu (noise) trông như hạt nổ TV vào ảnh gốc theo đúng chỉ dẫn của Gradient.
    
4. **Kết quả:**
    
    - **Mắt người:** Vẫn là con Gấu trúc (vì $\epsilon$ rất nhỏ).
        
    - **Mạng NN:** Nhìn thấy con Vượn (Gibbon) với độ tin cậy 99.3%.
        

### Tóm lại

FGSM "nhanh" (Fast) vì nó không cần lặp đi lặp lại. Nó chỉ cần tính đạo hàm **một lần duy nhất**, lấy dấu, rồi cộng thẳng vào ảnh. Nó chứng minh rằng mạng NN rất dễ vỡ trước các biến đổi tuyến tính đơn giản.