![[Pasted image 20260312221702.png]]

Slide này nói về **một cách interpret NN đơn giản hơn**:  
không sinh ảnh bằng gradient nữa, mà **tìm trong dataset xem ảnh nào kích hoạt neuron mạnh nhất**.

---

# Ý tưởng chính

Ta chọn **một filter / feature map** trong CNN.

Ví dụ layer cuối có tensor:

$$  
(5,5,256)  
$$

- 256 = số **feature maps / filters**
    
- mỗi filter có map (5 \times 5)
    

Ta chọn **một filter (k)**.

---

# Bước làm

Cho **tất cả ảnh trong dataset** chạy qua network.

Với mỗi ảnh (x), lấy activation của filter (k):

$$  
A_k(x)  
$$

Có thể lấy:

$$  
score(x) = \max A_k(x)  
$$

hoặc

$$  
score(x) = \frac{1}{HW}\sum_{i,j} A_k(x)_{ij}  
$$

---

# Sau đó

Sắp xếp:

$$  
\text{Top images} = \arg\max_x score(x)  
$$

tức là **những ảnh làm filter kích hoạt mạnh nhất**.

---

# Ý nghĩa

Nếu top ảnh đều giống nhau → ta đoán được filter học gì.

Ví dụ trong slide:

### Ví dụ 1

Top ảnh:

- áo sơ mi
    
- áo kẻ caro
    
- áo thun
    

→ suy ra filter đang detect: **shirt**

---

### Ví dụ 2

Top ảnh:

- cạnh tường
    
- cạnh gạch
    
- cạnh vật thể
    

→ filter detect: **edge**

---

# So với method trước (gradient ascent)

| Method          | Ý tưởng                                             |
| --------------- | --------------------------------------------------- |
| Gradient ascent | tạo **ảnh nhân tạo** làm neuron kích hoạt           |
| Dataset search  | tìm **ảnh thật trong dataset** làm neuron kích hoạt |

---

# Ưu điểm của dataset search

1. **ảnh thật → dễ hiểu hơn**
    
2. **không bị noise kỳ quái**
    
3. dễ implement
    

Chỉ cần:

```
for image in dataset:
    activation = model(image)
    score = activation[layer][filter]
```

---

# Nhược điểm

Chỉ thấy được **pattern đã có trong dataset**.

Nếu neuron học pattern lạ → dataset có thể **không chứa**.

---

# Tóm lại

Method này làm:

$$  
\text{Find } x \text{ in dataset that maximizes } a_j^{[l]}(x)  
$$
