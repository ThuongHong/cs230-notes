---
tags:
  - dl
  - stanford
  - cs230
class: cs230
date: 2026-03-02T16:38:00
---
https://youtu.be/s6JVGzABKho?si=qvfUaxFD-s7nyo3y

# AI Project Strategy

Tìm và đọc các tài liệu liên quan trước. Hãy đọc scan nhiều paper, sau đó chọn ra các paper có ý nghĩa nhất rồi mới đọc sâu.

Synthetic data cũng được, nhưng sẽ không ưu tiên.

Chú ý imbalanced dataset. Thầy Andrew gợi ý là độ lệch dưới 1:10 vẫn ổn.

Một số hacks của thầy Andrew (trong bài toán xây dựng model detect wake word):
- Thay vì cứ duplicate những positive examples, thì có thể extend độ dài đoạn audio đó lên...
- Bên cạnh đó, thầy nói về việc trong góc nhìn của paper thì train set, val set, test set thường có giả định là cùng phân phối. Nhưng trong thực tế thì chưa chắc...
- Cộng các audio background noise vào, có thể làm phong phú dataset. Nhưng cẩn thận, nếu ta chỉ cộng các noise vào các positive examples thì có thể model sẽ học sai: nếu audio nào có noise thì cho nó label positive