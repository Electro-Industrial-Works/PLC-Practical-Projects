# ⚡ Bài 2 – Bật/Tắt thiết bị bằng 1 nút (Toggle Control)

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/6636bde7-1899-455e-b7b2-848a991d002d" />

---

# 📘 Giới thiệu (Overview)

Bài 2 giới thiệu kỹ thuật:

* Bật/Tắt thiết bị bằng một nút duy nhất (Toggle Control)
* Phát hiện cạnh nhấn (ONE-SHOT)
* Lưu trạng thái bằng bit nhớ trung gian (M-bit)

Đây là kiểu điều khiển cực kỳ phổ biến trong công nghiệp và hệ thống điều khiển thực tế:

* Đèn ON/OFF
* Relay
* Motor nhỏ
* Nút chức năng trên HMI
* Push button điều khiển máy

---

## ⚠ Vấn đề thực tế

PLC quét chương trình liên tục theo chu kỳ scan.

Nếu sử dụng trực tiếp:

```text id="uk3xv8"
I0.0
```

để đảo trạng thái:

→ Khi người vận hành giữ nút lâu, ngõ ra có thể:

❌ Bật/Tắt liên tục
❌ Nhấp nháy theo scan PLC
❌ Hoạt động không ổn định

---

## ✅ Giải pháp của bài

Bài này sử dụng:

* ONE-SHOT (xung 1 scan)
* Bit nhớ trạng thái (M-bit)

để bảo đảm:

✅ Mỗi lần nhấn chỉ được tính đúng 1 lần.

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Nút bật/tắt đèn
* Toggle relay
* Điều khiển quạt nhỏ
* Nút chế độ Auto/Manual
* Push button trên operator panel
* Điều khiển ON/OFF bằng 1 nút

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Điều khiển ON/OFF bằng một nút duy nhất
* ✅ Tạo ONE-SHOT để phát hiện cạnh lên
* ✅ Lưu trạng thái bằng bit nhớ trung gian
* ✅ Chống việc giữ nút gây đổi trạng thái liên tục
* ✅ Tách logic trạng thái khỏi output thực tế
* ✅ Viết Toggle Logic theo chuẩn công nghiệp

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống chỉ sử dụng:

* 1 nút nhấn
* 1 đèn đầu ra

Mỗi lần nhấn:

| Trạng thái hiện tại | Trạng thái sau khi nhấn |
| ------------------- | ----------------------- |
| OFF                 | ON                      |
| ON                  | OFF                     |

→ Hệ thống hoạt động theo nguyên tắc:

```text id="bc6u2p"
Nhấn một lần → đổi trạng thái một lần
```

---

# ⚙ Cấu trúc xử lý của bài

Hệ thống được chia thành 4 bước:

| Bước | Chức năng               |
| ---- | ----------------------- |
| 1    | Phát hiện cạnh nhấn     |
| 2    | Tạo ONE-SHOT            |
| 3    | Toggle trạng thái       |
| 4    | Xuất trạng thái ra Q0.0 |

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Phát hiện cạnh lên (Rising Edge Detection)

PLC so sánh:

* trạng thái hiện tại của nút
* trạng thái ở scan trước

Khi:

```text id="7xq2jf"
I0.0 : 0 → 1
```

→ PLC tạo:

```text id="x2f5ru"
ONE_SHOT (M0.5)
```

---

## ✅ Ý nghĩa

ONE_SHOT chỉ tồn tại:

* đúng 1 chu kỳ quét (1 scan)

→ dù người vận hành giữ nút lâu:

PLC vẫn chỉ xử lý:

* đúng 1 lần nhấn

---

# 2️⃣ Toggle trạng thái đèn

Khi:

```text id="r6v1mq"
ONE_SHOT = 1
```

PLC sẽ đảo trạng thái:

```text id="z8u5qn"
M0.0
```

---

## Logic Toggle

| Trạng thái cũ | Trạng thái mới |
| ------------- | -------------- |
| 0             | 1              |
| 1             | 0              |

---

# 3️⃣ Xuất ra ngõ ra thực

Ngõ ra thực tế:

```text id="u0w8pk"
Q0.0
```

luôn bám theo:

```text id="r4k7xb"
M0.0
```

---

## ✅ Ý nghĩa

Việc tách:

* Logic State
* Physical Output

giúp chương trình:

* dễ mở rộng
* dễ debug
* đúng chuẩn công nghiệp

---

# 4️⃣ Ghi nhớ trạng thái nút scan trước

PLC sử dụng:

```text id="g5p9yh"
M1.0
```

để lưu trạng thái cũ của nút nhấn.

---

## ✅ Mục đích

Phục vụ:

* Rising Edge Detection
* ONE-SHOT Generation

---

# 5️⃣ Giữ nút không làm đèn đổi liên tục

Nếu người vận hành:

* giữ nút lâu

→ sẽ không xuất hiện cạnh mới.

→ PLC không tạo ONE_SHOT mới.

→ đèn không bị nhấp nháy liên tục.

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Tên biến   | Địa chỉ | Kiểu | Ghi chú               |
| ---------- | ------- | ---- | --------------------- |
| PB_LOCAL   | I0.0    | BOOL | Nút nhấn              |
| LAMP_STATE | M0.0    | BOOL | Trạng thái đèn        |
| ONE_SHOT   | M0.5    | BOOL | Xung 1 scan           |
| PB_PREV    | M1.0    | BOOL | Trạng thái scan trước |
| LAMP       | Q0.0    | BOOL | Đèn output            |

> ⚠ Trong quá trình thực hành, có thể bổ sung thêm biến nhớ để mở rộng logic điều khiển.

---

# 🪜 Ladder Logic

<img width="728" height="365" alt="image" src="https://github.com/user-attachments/assets/b8ff33fb-ae5e-41dd-a7df-165ec705ac58" />

<img width="733" height="582" alt="image" src="https://github.com/user-attachments/assets/c4439d21-ee3a-4df6-8630-11ee3b84cc7a" />
---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng logic rất phổ biến trong:

* Toggle Control
* Push Button Interface
* Operator Panel
* Human Machine Interaction
* Relay Toggle System

---

# 📂 File dự án


<img width="331" height="68" alt="image" src="https://github.com/user-attachments/assets/249ebad5-556d-437b-ab77-ef645681f47c" />

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Viết Toggle Logic chuẩn
* ✅ Tạo ONE-SHOT
* ✅ Phát hiện cạnh lên
* ✅ Tách Logic và Output
* ✅ Chống giữ nút gây lỗi
* ✅ Viết Ladder Logic ổn định hơn

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Debounce Filtering
* Double-click detection
* Long press detection
* Toggle nhiều output
* HMI push button
* Lockout chống nhấn nhanh

---


# 📚 PLC Practical Projects Series

Series bao gồm:

* Start/Stop
* Timer
* Counter
* Toggle Logic
* Debounce
* One-Shot
* Alarm System
* Industrial Automation

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để hỗ trợ phát triển thêm nhiều project PLC thực tế 🚀

---




