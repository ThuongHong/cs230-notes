---
tags:
  - dl
  - stanford
  - cs230
class: cs230
date: 2026-03-12T14:29:00
---
https://youtu.be/Ozb1AR_F5MU?si=bNP5kN_hNx8K1vJj

![[Pasted image 20260312143610.png]]
**Answers fall into 4 buckets**
- Training & scaling (los curves, grad norms, LR, MoE routing, scaling laws) -> model được train thế nào
- Representations & internals (attention heads, embeddings, neuron behavior) -> bên trong model làm gì
- Data & distribution (drift, contamination domain mix changes) -> dữ liệu ảnh hưởng ra sao
- Capabilities & safety (benchmarks, adversarial evals, red-team reports) -> model làm được gì và có nguy hiểm không

---
## II. CNN Interpretation

### A. saliency maps

![[Pasted image 20260312145215.png]]

**Tạo saliency map:**
1. Forward pass: tính score của class dog
2. Backprop: tính $\frac{\partial s_{dog}}{\partial x}$. Tức là ta cần tìm nếu thay đổi pixel một chút thì score class thay đổi bao nhiêu.
3. Lấy magnitude của gradient
4. Hiển thị thành heatmap

**Kết quả:** 
- vùng sáng = model đang nhìn vào
- vùng tối = model bỏ qua

Saliency map được tính trên pre-softmax score (logit). Vì softmax làm gradient bị "loãng". Bởi vì khi lấy softmax, pixel importance bị trộn giữa nhiều class. Trong khi đó ta chỉ quan tâm pixel nào làm class dog mạnh lên.

**Nhược điểm của saliency maps:**
- Saliency map chỉ cho biết pixel nào ảnh hưởng mạnh tới output (pixel-level importance)
- Nó không cho ta hiểu được ngữ nghĩa (semantic meaning) của những vùng đó. Tức là ta biết pixel nào quan trọng, nhưng kông biết nó tương ứng với object/feature nào
- Saliency map thường rất noisy và khó diễn giải

### A'. integrated gradients
 ![[Pasted image 20260312151720.png]]

**Saliency map** chỉ dùng 1 gradient tại đúng input cuối
**Integrated Gradients (IG)** dùng nhiều gradient trên đường baseline đến input

Giải thích [[Intergrated Gradients]]

---
### B. occlusion sensitivity

Ta che (occlude) từng vùng của ảnh rồi xem prediction của model thay đổi bao nhiêu.
![[Pasted image 20260312153257.png]]
![[Pasted image 20260312153420.png]]

---
### C. class activation maps (CAM)

![[Pasted image 20260312154136.png]]

![[Pasted image 20260312210827.png]]

#### 1. Output của last CONV layer

Giả sử lớp conv cuối tạo ra **K feature maps**:
$$ 
f_k(x,y), \quad k = 1,\dots,K  
$$
- (x,y) là vị trí pixel trong map
- kích thước mỗi map: (H \times W)

#### 2. Global Average Pooling (GAP)

Lấy **trung bình mỗi feature map**:
$$ 
z_k = \frac{1}{HW} \sum_{x=1}^{H} \sum_{y=1}^{W} f_k(x,y)  
$$
Ta thu được vector:
$$
z = (z_1, z_2, ..., z_K)  
$$
#### 3. Tính **class score**

Score của class (c):
$$
s_c = \sum_{k=1}^{K} w_{c,k} , z_k  
$$
Tức là:
$$
s_c = \sum_{k=1}^{K} w_{c,k}  
\left(  
\frac{1}{HW} \sum_{x,y} f_k(x,y)  
\right)  
$$
Ý nghĩa:

```text
score = sum(weight × trung bình feature map)
```


#### 4. Tính **Class Activation Map (CAM)**

Heatmap của class (c):
$$  
CAM_c(x,y) =  
\sum_{k=1}^{K} w_{c,k} , f_k(x,y)  
$$
Ý nghĩa:

```text
heatmap = sum(weight × feature map)
```

#### 5. Quan hệ giữa score và CAM

Nếu lấy trung bình CAM:
$$
\frac{1}{HW} \sum_{x,y} CAM_c(x,y)  
$$

thì ta được:
$$
s_c  
$$
Nói cách khác:
$$
s_c = avg(CAM_c)  
$$

#### 6. Tóm tắt ngắn gọn

**Score:**
$$
s_c = \sum_k w_{c,k} , avg(f_k)  
$$

**Heatmap:**
$$  
CAM_c(x,y) = \sum_k w_{c,k} f_k(x,y)  
$$

Nếu nhìn kỹ thì sẽ thấy một insight rất hay:

> **Classifier của CNN thực chất là cộng các “object-part detector maps” lại với trọng số khác nhau.**

Đó là lý do CAM thường highlight **tai chó, mặt chó, thân chó** thay vì toàn bộ ảnh.

---

### D. gradient ascent (class model visualization)

![[Pasted image 20260312213830.png]]
![[Pasted image 20260312214029.png]]

Giải thích [[gradient ascent]]

![[Pasted image 20260312215014.png]]

Ta cũng có thể chọn bất kì neuron nào. Sau khi optimize, ta được input image làm neuron đó kích hoạt mạnh nhất => giúp hiểu neuron đang detect cái gì.

---

### E. dataset search

![[Pasted image 20260312221702.png]]

Giải thích [[dataset search]]

![[Pasted image 20260312224335.png]]
#### **Ta thấy những bức ảnh ở trên bị crop, vì sao?**

Mỗi vị trí trong **feature map** tương ứng với **một vùng trong ảnh gốc** (gọi là **receptive field**).

Nên nếu activation lớn tại: $A_k(i,j)$

→ nghĩa là **filter $k$ phát hiện feature mạnh ở vùng ảnh tương ứng với vị trí (i,j)**.

Ta chỉ cần:
1. tìm vị trí activation lớn trong feature map
2. map vị trí đó về **vùng ảnh gốc (receptive field)**
3. **crop vùng đó**

---

### F. deconvolution

Giải thích [[deconvolution]]

![[Pasted image 20260313210518.png]]

Dùng [[deconvolution]] để kiểm tra xem feature map học được gì
![[Pasted image 20260313211412.png]]
Ta có thể làm điều này ở layer nào cũng được
![[Pasted image 20260313213039.png]]

Ở bước unpool, ta phải lưu switches để có thể unpool
![[Pasted image 20260313211345.png]]

**ReLU**
![[Pasted image 20260313211819.png]]
**ReLU Forward**
- Điều kiện giữ: (x > 0)
- Ý nghĩa: giữ activation dương, chặn tín hiệu âm
- Dùng khi: forward pass của CNN khi train/inference

**ReLU Backward (Backprop)**
- Điều kiện giữ: forward activation > 0
- Ý nghĩa: gradient chỉ đi qua neuron đã kích hoạt ở forward
- Dùng khi: huấn luyện mạng (tính gradient để update weight)

**ReLU DeconvNet**
- Điều kiện giữ: gradient > 0
- Ý nghĩa: giữ tín hiệu làm tăng activation, giúp thấy pattern neuron thích
- Dùng khi: visualize / interpret CNN (DeconvNet)

---

![[Pasted image 20260313215816.png]]

---

## II. Modern representation analysis

![[Pasted image 20260313220819.png]]

---
## III. Training & scaling diagnostics

![[Pasted image 20260313222354.png]]

---
## V. Capabilities & safety dashboards

![[Pasted image 20260313225912.png]]

---
## VI. Data diagnostics

![[Pasted image 20260313225033.png]]

---
## Summary

![[Pasted image 20260313225123.png]]