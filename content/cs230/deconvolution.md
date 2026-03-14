
![[Pasted image 20260313210148.png]]

**Deconvolution** trong CNN thực ra **không phải phép nghịch đảo thật của convolution**. Trong deep learning người ta thường dùng nó để **tăng kích thước feature map**.

Tên chính xác hơn là **Transposed Convolution**.

---

## 1. Ý tưởng chính

Convolution bình thường:

```
Input (lớn)  --conv-->  Output (nhỏ)
```

Ví dụ:

```
5×5  --conv-->  3×3
```

Còn **deconvolution** thì ngược lại:

```
Input (nhỏ)  --deconv-->  Output (lớn)
```

Ví dụ:

```
3×3  --deconv-->  5×5
```

Nên nó thường dùng trong:

- **Image generation**
    
- **Segmentation**
    
- **Super-resolution**
    
- **GAN**
    
- **Autoencoder decoder**
    

---

## 2. Cách nó thực sự hoạt động

Giả sử:

```
Input: 2×2
Kernel: 3×3
Stride = 2
```

Transposed convolution làm 2 bước:

### Bước 1 — chèn zero (zero insertion)

Nếu stride = 2:

```
1 2
3 4
```

chèn 0 vào giữa →

```
1 0 2
0 0 0
3 0 4
```

---

### Bước 2 — convolution bình thường

Sau đó **convolve kernel lên ma trận đã chèn zero**.

Kết quả:

```
output lớn hơn input
```

---

## 3. Vì sao gọi là _transposed_ convolution

Nếu viết convolution thành **matrix multiplication**:

```
y = W x
```

thì transposed convolution chính là:

```
x = Wᵀ y
```

tức là **dùng ma trận chuyển vị của convolution**.

Nên mới gọi là:

```
Transposed Convolution
```

chứ **không phải inverse convolution**.

---

## 4. So sánh nhanh

|Operation|Kích thước|
|---|---|
|Convolution|giảm size|
|Pooling|giảm size|
|**Transposed convolution**|**tăng size**|

---

![[Pasted image 20260313210251.png]]
## 5. Khác với Sub-pixel convolution

Hai cái đều **upsample** nhưng cách làm khác:

### Transposed convolution

- chèn **zero vào input**
    
- rồi **convolution**
    

### Sub-pixel convolution

- **convolution trước**
    
- sau đó **rearrange channel → spatial**
    

tức là:

```
feature map channels
      ↓
pixel shuffle
      ↓
image resolution tăng
```

---

## 6. Một lỗi nổi tiếng: checkerboard artifact

Deconvolution dễ tạo pattern kiểu:

```
▢ ▢ ▢ ▢
 ▢ ▢ ▢
▢ ▢ ▢ ▢
```

gọi là **checkerboard artifact**.

Vì vậy nhiều model hiện đại dùng:

- **upsample + conv**
    
- **pixel shuffle**
    

thay cho deconv.

---

![[Pasted image 20260313210532.png]]

💡 Tóm lại:

**Deconvolution trong CNN = transposed convolution**

```
nhỏ → lớn
upsampling
bằng cách chèn zero rồi convolution
```
