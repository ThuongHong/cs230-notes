
Giải thích **Integrated Gradients (IG)** theo cách dễ hiểu nhất.

---

# 1. Vấn đề cần giải quyết

Ta có một neural network:

```text
image → model → dog score
```

Ta muốn biết:

> pixel nào trong ảnh làm **dog score tăng lên**.

Cách đơn giản (saliency map):

[  
\frac{\partial s_{dog}}{\partial x}  
]

Nhưng cách này có vấn đề:

- gradient **noisy**
    
- **gradient saturation**
    
- chỉ nhìn **một điểm duy nhất**
    

---

# 2. Ý tưởng của Integrated Gradients

Thay vì nhìn **chỉ tại input cuối**, IG hỏi:

> Nếu ta **từ từ xây dựng ảnh này từ trạng thái “không có gì”**, thì pixel nào làm score tăng lên?

---

# 3. Chọn baseline

Ta chọn một **baseline** (x'):

ví dụ:

```
ảnh đen hoàn toàn
```

Nó đại diện cho **không có thông tin**.

---

# 4. Đi từ baseline → ảnh thật

Ta tạo các ảnh trung gian bằng **nội suy tuyến tính**:

[  
x(\alpha) = x' + \alpha (x - x')  
]

- (\alpha = 0) → baseline
    
- (\alpha = 1) → ảnh thật
    

Ví dụ:

```text
baseline → ảnh hơi hiện → ảnh mờ → ảnh gần thật → ảnh thật
```

---

# 5. Ở mỗi bước ta làm gì

Với mỗi ảnh trung gian:

```
x(α)
```

ta tính:

[  
\frac{\partial F(x(\alpha))}{\partial x}  
]

tức là:

> gradient của class score theo pixel.

---

# 6. Cộng tất cả gradient lại

Sau đó ta **tích phân (hoặc lấy trung bình)** tất cả gradient trên đường đó.

Kết quả cuối:

[  
IG_i(x) =  
(x_i - x_i') \int_0^1  
\frac{\partial F(x' + \alpha(x-x'))}{\partial x_i} d\alpha  
]

Ý nghĩa:

> pixel (i) đóng góp bao nhiêu vào việc tăng score từ baseline → input.

---

# 7. Trực giác

Saliency map:

```
chỉ hỏi tại điểm cuối
```

Integrated Gradients:

```
quan sát toàn bộ quá trình từ "không có gì" → "ảnh thật"
```

---

# 8. Ví dụ đơn giản

Giả sử pixel tai chó làm model nhận ra **dog**.

Khi ảnh dần hiện ra:

```
baseline
→ bắt đầu thấy tai
→ dog score tăng
→ thấy toàn bộ chó
```

IG sẽ gán **attribution cao cho pixel ở tai**.

---

# 9. Tính chất hay của IG

Integrated Gradients có property gọi là **completeness**:

[  
\sum_i IG_i = F(x) - F(x')  
]

Nghĩa là:

> tổng đóng góp của tất cả pixel = chênh lệch output giữa baseline và input.

---

# 10. Tóm tắt cực ngắn

Integrated Gradients:

1. chọn **baseline**
    
2. đi từ baseline → input
    
3. tính **gradient ở nhiều điểm trên đường đó**
    
4. **tích phân các gradient** để tính contribution của từng feature
    

---

Nếu cha học interpretability CNN thì có một insight quan trọng:

> Saliency map nhìn **gradient tại một điểm**, còn Integrated Gradients nhìn **gradient của cả một con đường trong không gian input** — nên attribution ổn định hơn nhiều.