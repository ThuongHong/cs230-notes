---
tags:
  - dl
  - stanford
  - cs230
class: cs230
date: 2026-02-28T22:54:00
---
https://youtu.be/4E27qlfYw0A?si=dkOdWa7PtPOcLEmW

# Deep Reinforcement Learning

## Motivation

![[Pasted image 20260228165738.png]]


![[Pasted image 20260228170357.png]]
Một số khái niệm: action, state, observation, reward, transition
Trong đó state và observation thỉnh thoảng có thể dùng như nhau.

Vậy vì sao người ta lại tách 2 khái niệm đó ra riêng? 
> Bởi vì trong nhiều trường hợp state có thể không rõ ràng với agent. Ví dụ như trong chess hay cờ vây thì state là bàn cờ nên agent dễ dlâu lâuàng quan sát được luôn. Nhưng mà nếu trong mấy game kiểu LoL, thì map nó có cái sương mù nên ta sẽ không thể quan sát được hết map.

## Recycling is good: an intro to RL

![[Pasted image 20260228174748.png]]

Giải thích [[discounted return]]

Nếu gamma ở trên cho bằng 1 (tức là agent sẽ ưu tiên reward sau cùng, không greedy), thì dễ

Nhưng nếu gamma < 1 thì khó hơn tí. 

![[Pasted image 20260228190119.png]]
### Giải thích:

Cách tính trong hình dựa trên phương trình Bellman để tìm giá trị cho bảng **Q-table**. Bảng này có 5 hàng (tương ứng 5 trạng thái S1-S5) và 2 cột (Cột 1: đi sang Trái, Cột 2: đi sang Phải).

Công thức tổng quát được áp dụng ở đây là:

$$Q(S_{hiện\_tại}, hành\_động) = r_{tiếp\_theo} + \gamma \times \max Q(S_{tiếp\_theo})$$

Trong đó:

- $r_{tiếp\_theo}$: Phần thưởng nhận được khi bước vào trạng thái tiếp theo.
    
- $\gamma$ (hệ số chiết khấu) = $0.9$.
    
- $\max Q(S_{tiếp\_theo})$: Giá trị lớn nhất (tốt nhất) có thể đạt được ở bước tiếp theo.
    

**Giải thích chi tiết các phép tính trên sơ đồ cây ("How?"):**

- **Từ S4 đi sang Phải (đến S5):** Bước vào S5 được thưởng ngay +10 (đây là đích, không đi tiếp).
    
    $\rightarrow Q(S4, Phải) = 10$
    
- **Từ S3 đi sang Phải (đến S4):** Bước vào S4 được thưởng +1. Từ S4, hành động tốt nhất là đi tiếp sang Phải để được 10.
    
    $\rightarrow Q(S3, Phải) = 1 + 0.9 \times 10 = 10$
    
- **Từ S2 đi sang Phải (đến S3):** Bước vào S3 được thưởng 0. Từ S3, hành động tốt nhất là đi sang Phải với giá trị đã tính là 10.
    
    $\rightarrow Q(S2, Phải) = 0 + 0.9 \times 10 = 9$
    
- **Từ S3 đi sang Trái (quay lại S2):** Bước vào S2 được thưởng 0. Từ S2, hành động tốt nhất là đi sang Phải với giá trị đã tính là 9.
    
    $\rightarrow Q(S3, Trái) = 0 + 0.9 \times 9 = 8.1$
    
- **Từ S2 đi sang Trái (đến S1):** Bước vào S1 được thưởng +2 (đích đến, dừng lại).
    
    $\rightarrow Q(S2, Trái) = 2$
    

Các kết quả này khớp hoàn toàn với các con số trong ma trận $Q$-table ở góc phải.

---

### Bellman equation

![[Pasted image 20260301143657.png]]

Vấn đề với Q-table là nếu không gian state và action quá lớn, nó sẽ tốn bộ nhớ

## Deep Q-Learning

Ý tưởng: Thay vì tính toán Q-table thì mình dùng 1 NN để thay thế
 ![[Pasted image 20260301145254.png]]
![[Pasted image 20260301165617.png]]![[Pasted image 20260301165636.png]]
 
## Application: Breakout (Atari)

![[Pasted image 20260301175245.png]]

![[Pasted image 20260301175947.png]]

## Tips to train Deep Q-Network

![[Pasted image 20260302005034.png]]

### 1️⃣ Preprocessing

Ý nghĩa: xử lý state trước khi đưa vào mạng.

Ví dụ với game Atari:

- Resize ảnh
    
- Convert sang grayscale
    
- Stack nhiều frame lại
    

Mục đích:

- Giảm dimension
    
- Làm input ổn định hơn
    
- Giữ thông tin chuyển động
    

Nếu không preprocessing tốt → network học rất chậm hoặc không học được.

---

### 2️⃣ Detect terminal state

Phải biết khi nào episode kết thúc.

Nếu s' là terminal thì:

[  
y = r  
]

Không có phần ( \gamma \max Q(s',a') ) nữa.

Vì không có tương lai sau trạng thái kết thúc.

Nếu không xử lý đúng terminal → Q-value sẽ bị sai lệch.

---

### 3️⃣ Experience Replay

- Lưu các transition (s, a, r, s')
    
- Random sample mini-batch để train
    

Tác dụng:

- Phá correlation giữa các bước liên tiếp
    
- Tăng ổn định
    
- Tái sử dụng dữ liệu nhiều lần
    

Đây là một trong hai thứ cứu DQN khỏi diverge.

---

### 4️⃣ Epsilon-greedy action

Không phải lúc nào cũng chọn action có Q lớn nhất.

Với xác suất ε:

- Chọn random action
    

Mục đích:

- Exploration
    
- Tránh kẹt ở local optimum
    
- Đảm bảo khám phá môi trường
    

Thường ε giảm dần theo thời gian.

---

# Tóm lại bản chất

4 thứ này giải quyết 4 vấn đề khác nhau:

- Preprocessing → xử lý input
    
- Terminal detection → đúng công thức Bellman
    
- Replay → ổn định training
    
- Epsilon-greedy → đảm bảo exploration
    

Nếu bỏ một trong mấy cái này, DQN dễ học dở hoặc fail hẳn.

---
## RLHF

![[Pasted image 20260302115013.png]]

Trước đó, ta phải train Reward Model để thay thế cho đánh giá của con người ở RLHF:

![[Pasted image 20260302115335.png]]