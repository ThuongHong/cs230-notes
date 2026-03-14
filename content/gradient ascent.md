![[Pasted image 20260312213830.png]]

# Bài toán

Ta muốn tìm một ảnh (x) làm model tin rằng đó là **dog**.

Giữ **weights của network cố định**, chỉ **update input image**.

---

# Loss function

$$  
L(x) = s_{\text{dog}}(x) - \lambda |x|_2^2  
$$

Trong đó:

- $s_{\text{dog}}(x)$ = score của class **dog** trước softmax
    
- $\lambda |x|_2^2$ = regularization để ảnh không thành noise
    

---

# Gradient ascent trên input

Ta **maximize loss** bằng gradient ascent:
$$  
x \leftarrow x + \alpha \frac{\partial L}{\partial x}  
$$
Trong đó:
- $\alpha$ = learning rate
- $\frac{\partial L}{\partial x}$ = gradient của loss theo pixel


---

# Thuật toán

Lặp lại nhiều lần:

1. Forward pass:
$$  
s_{\text{dog}}(x)  
$$

2. Tính loss:
$$  
L(x) = s_{\text{dog}}(x) - \lambda |x|_2^2  
$$

3. Backprop:
$$  
\frac{\partial L}{\partial x}  
$$

4. Update ảnh:
$$  
x \leftarrow x + \alpha \frac{\partial L}{\partial x}  
$$

---

# Mục tiêu cuối cùng

Bài toán tối ưu:

$$  
\max_x s_{\text{dog}}(x)  
$$

(với regularization để ảnh nhìn tự nhiên hơn)

---

# Kết quả

Sau nhiều iteration:

- bắt đầu từ **noise**
    
- ảnh dần xuất hiện **pattern mà network nghĩ là “dog”**.