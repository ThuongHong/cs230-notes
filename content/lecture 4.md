---
tags:
  - dl
  - stanford
  - cs230
class: cs230
date: 2026-02-12T14:48:00
---
https://youtu.be/aWlRtOlacYM?si=6xDP18VnPlMXpRbL

# Adversarial Robustness

3 waves of adversarial attack
![[Pasted image 20260212150224.png]]
## A. Attacking a network with adversarial examples

**Goal**: Ta có 1 model được train trên dataset ImageNet. Hãy tìm 1 tấm ảnh mà khi cho vào model để predict, nó sẽ ra được **iguana** 

Tóm tắt ý tưởng trong video: Ta định nghĩa loss $L(\hat{y},y)$, thể hiện sự sai khác của dự đoán so với cái target ta cần ($y_{iguana}$). Dựa vào loss ta điều chỉnh từng pixel của tấm ảnh, cuối cùng ta sẽ có 1 tấm ảnh sẽ luôn được dự đoán là **iguana**.

Ta sẽ không biết được liệu tấm ảnh đó có giống như iguana không. Bởi vì 
![[Pasted image 20260212160703.png]]


**Goal**: Ta có 1 model được train trên dataset ImageNet. Hãy tìm 1 tấm ảnh hiển thị 1 con mèo nhưng được phân loại là iguana.

Tóm tắt: Cũng tương tự như trên nhưng ta sẽ thêm các điều kiện để ép cho tấm hình trông giống như con mèo, cũng như là ta sẽ bắt đầu điều chỉnh từ tấm ảnh con mèo luôn (thay vì bắt đầu từ 1 tấm ảnh random).
![[Pasted image 20260212164020.png]]
![[Pasted image 20260212164209.png]]

**black-box attack:** tấn công model khi không biết bên trong nó hoạt động thế nào:

## B. Why are neural networks vulnerable to adversarial examples?

**What makes model sensitive?**
Ban đầu các nhà nghiên cứu cho rằng NN nhạy cảm là do tính phi tuyến cao của nó, những thay đổi nhỏ ở input có thể dẫn đến biến đổi phi tuyến ở output. Cái này không đúng.
Thực tế là [[Mạng NN có tính tuyến tính]] cao. Ngay cả khi NN sử dụng các activation function phi tuyến, khi xem xét nó từ input đến logit, nó thực sự là tuyến tính. 
Lí do thực tế là do tính đa chiều của vấn đề.
![[Pasted image 20260213100545.png]]


### [[Fast Gradient Sign Method]]
![[Pasted image 20260213100741.png]]


## C. Defenses against adversarial examples

![[Pasted image 20260213172326.png]]

- [[Constitutional AI]]

## D. Backdoor and Data Poisoning Attacks

![[Pasted image 20260213172752.png]]
Cái này đầu độc AI từ nguồn dữ liệu training (căng)

## E. Prompt Injection

![[Pasted image 20260214001350.png]]

 

# Generative Models

  GAN và Diffusion Models đều là self-supervised, nhưng khác với contrastive learning:
  - Contrastive learning -> embedding
  - GAN và Diffusion Models -> generative tasks
## GAN (Generative Adversarial Networks)


![[Pasted image 20260214150611.png]]

![[Pasted image 20260214153244.png]]

**Vấn đề với việc training GAN ở trên** (cold start): Vào thời điểm ban đầu, D sẽ dễ dự đoán 1 bức ảnh bất kì nào đó mà G tạo ra là fake, dẫn đến G học được rất ít bởi vì gradient nhỏ (vanishing gradient).

Vì vậy ta sẽ giải quyết bằng hàm loss khác:
![[Pasted image 20260214155118.png]]
![[Pasted image 20260214155538.png]]

Chi tiết về [[Heuristic giúp Generator học được ở giai đoạn đầu]]

---
**Câu hỏi**: Liệu G có thể tạo ra một bức ảnh về object cụ thể nào đó, hay là nó chỉ tạo ra bức ảnh đủ để đánh lừa D?
**Trả lời:**

Trong GAN nguyên bản (Vanilla GAN), câu trả lời là: **Nó chỉ tạo ra bức ảnh đủ để đánh lừa D.**

Cụ thể:

1. **Không kiểm soát được:** Vì đầu vào $z$ là nhiễu ngẫu nhiên, Generator sẽ sinh ra một ảnh ngẫu nhiên nằm trong tập dữ liệu (ví dụ: train trên bộ MNIST thì nó có thể sinh ra số 1 hoặc số 9 tùy ý, bạn không thể ra lệnh cho nó).
    
2. **Rủi ro "Mode Collapse":** G có thể trở nên "lười biếng" bằng cách chỉ tạo ra **duy nhất một tấm ảnh** mà nó thấy D dễ bị lừa nhất (ví dụ: chỉ toàn tạo ra số 1) và bỏ qua các số khác.
    

**Giải pháp:** Để tạo ra object cụ thể (ví dụ: "hãy tạo số 7"), người ta phải dùng biến thể **Conditional GAN (cGAN)** – tức là đưa thêm nhãn (label) vào cho cả G và D trong lúc huấn luyện.

---

**Thú vị:**
![[Pasted image 20260214161741.png]]


## Diffusion Models

### A. Basic Principles

**Vấn đề với GAN**:
![[Pasted image 20260214173307.png]]

#### Forward process

![[Pasted image 20260214174442.png]]
 
 **Câu hỏi**: Tại sao ta lại muốn nhiễu $\epsilon_t$ lấy từ Gaussian? 
 **Trả lời**: Để đơn giản hóa quá trình training 

Quá trình này sẽ tự tạo label từ data ảnh, mỗi entry gồm:
- noisy image
- index of time step
- cumulative noise added $\epsilon$
-> self-supervised learning

#### Reverse process (denoising)

![[Pasted image 20260214175441.png]]

### B. Loss function and training

![[Pasted image 20260214181855.png]]

**Câu hỏi**: What's the order of magnitude of how much images we need?
**Trả lời**:  With this kind of task (generative task), the output's dimension is higher than the input's dimension. So just feed the model more data than its capacity.

#### Denoising Schedule (Noise Schedule)
![[Pasted image 20260214183103.png]]

### C. Sampling
 ![[Pasted image 20260214183923.png]]

Model sẽ random ra ảnh output, ta sẽ không biết được liệu nó ra ảnh con chó, hay là con mèo...

Tóm tắt: Ta sẽ cho model 1 ảnh noisy, cho nó biết bắt đầu từ time step $T$ (để lặp lại thuật toán T lần). Ở mỗi bước, model sẽ ước lượng noise hiện tại, từ đó có được ảnh ở time step trước, lặp đủ $T$ lần.

Ta thấy nhược điểm là tính toán rất tốn kém. Nhưng được cái là càng tính đến các step sau, thì tính toán càng đơn giản.

### D. Latent Diffusion

![[Pasted image 20260214185404.png]]

Trong thực tế, model thường được sampling dưới 1 điều kiện nào đó ở phương thức khác (ví dụ như mình bắt ChatGPT tạo ảnh) để có được kết quả mong muốn.

**Về tạo video**
![[Pasted image 20260214185851.png]]
Về cơ bản thì ta sẽ cần lưu nhiều thông tin hơn trong mỗi latent vector (lưu thêm thông tin thời gian....)