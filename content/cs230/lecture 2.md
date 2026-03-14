---
tags:
  - dl
  - stanford
  - cs230
class: cs230
date: 2026-02-08T08:30:00
---
https://youtu.be/DNCn1BpCAUY?si=-EYJEqrF6bDjdt1y
# recap
- model = architecture + params
- how does the model learn? -> GD optimization...
-  things that can change in NN:
	- activation function
	- optimizer
	- hyperparameters
- lưu ý tương quan giữa model capacity và lượng data
- feature engineering: manual - humans specify features
- feature learning: automatic - the network figures out features during training
- embedding = encoding + meaning (where closenehss = similarity): nghĩa là distance giữa 2 encoding có ý nghĩa. Ví dụ 2 encoding gần nhau sẽ cho biết gì, xa nhau cho biết gì?

# Supervised Learning
 - Trong mấy bài binary classification, input thường lớn hơn output. Ngược lại: generative models,...
 - triplet loss: loss function trong đó có 1 anchor input được so với 1 positive input (mẫu này có label = anchor input) và 1 negative input. Ta cần tối thiểu hóa dist anchor và positive, tối đa hóa dist anchor và negative.
- Phân biệt: face identification >< face verification

# Self-Supervised Learning & Weakly Supervised Learning
 - Ví dụ trong task image classification, nhưng không có label. Người ta sẽ "kéo" những ảnh giống nhau lại gần, "đẩy" những ảnh khác nhau ra xa (**contrastive learning**).    
 - **Emergent behaviors** = năng lực bất ngờ xuất hiện khi scale lớn, dù objective huấn luyện rất đơn giản![[Pasted image 20260209215558.png]]
 - **weakly supervised**: train model with imperfect or automatically generated labels instead of precise human ones. We can learn a **shared embedding space** across modalities using text as the central pivot.