# ⚡ Bài 5 – Lọc chống dội và tạo xung sạch (ONE-SHOT) cho một nút bấm

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/1d2a4b7f-3920-4952-82da-1e3dc390f307" />

---

# 📘 Giới thiệu (Overview)

Bài 5 mở rộng kỹ thuật điều khiển bật/tắt bằng 1 nút nhấn (Toggle Control) bằng cách bổ sung hai cơ chế cực kỳ quan trọng trong hệ thống thực tế:

* ✅ Debounce Filtering (lọc chống dội)
* ✅ ONE-SHOT (xung sạch 1 scan)

Trong thực tế, các nút cơ khí thường bị:

* rung tiếp điểm
* nhiễu tín hiệu
* tạo nhiều cạnh ON/OFF trong vài mili-giây đầu

Nếu không xử lý đúng:

❌ Đèn sẽ bật/tắt liên tục
❌ Toggle sai trạng thái
❌ PLC nhận nhiều lần nhấn giả

Bài này hướng dẫn cách:

* dùng TON để lọc dội
* tạo xung sạch
* toggle chính xác và ổn định

theo đúng chuẩn lập trình PLC công nghiệp.

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Nút ON/OFF trên tủ điện
* Push Button điều khiển motor
* Nút nhấn operator
* Hệ thống chịu rung động
* Toggle control trong machine panel
* Start/Stop bằng 1 nút

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Hiểu hiện tượng Contact Bounce
* ✅ Dùng TON để debounce nút nhấn
* ✅ Tạo ONE-SHOT bằng Rising Edge Detection
* ✅ Toggle trạng thái bằng M-bit
* ✅ Tách logic khỏi Output Q
* ✅ Đảm bảo toggle chính xác khi nhấn nhanh hoặc giữ nút

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống hoạt động bằng một nút duy nhất:

| Hành động  | Kết quả         |
| ---------- | --------------- |
| Nhấn lần 1 | Đèn bật         |
| Nhấn lần 2 | Đèn tắt         |
| Nhấn lần 3 | Đèn bật         |
| ...        | Toggle liên tục |

---

# ⚠ Vấn đề thực tế

Nút cơ khí thật thường bị:

* Contact Bounce
* Rung tiếp điểm
* Nhiều cạnh giả trong 5–20 ms đầu

Nếu không xử lý:

❌ Toggle nhiều lần
❌ Bật rồi tắt ngay
❌ Hoạt động không ổn định

---

# ✅ Giải pháp của bài

Hệ thống xử lý theo 3 bước:

| Bước | Chức năng         |
| ---- | ----------------- |
| 1    | Debounce bằng TON |
| 2    | Tạo ONE_SHOT      |
| 3    | Toggle M0.0       |

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Debounce Filtering

Tín hiệu nút nhấn:

```text id="d9h2ra"
I0.0
```

được đưa qua:

```text id="8sq7g9"
TON T33
```

Preset:

```text id="5x7lhm"
30–50 ms
```

Chỉ khi nút giữ ổn định đủ thời gian:

→ T33 bật

→ tạo tín hiệu:

```text id="xv0gfj"
PB_DEBOUNCED
```

---

### ✅ Ý nghĩa

PLC sẽ bỏ qua:

* rung tiếp điểm
* nhiễu ngắn
* cạnh giả

---

## 2️⃣ Tạo ONE-SHOT

PLC so sánh:

* trạng thái hiện tại
* trạng thái trước đó

Khi:

```text id="8mtnje"
PB_DB: 0 → 1
```

→ PLC tạo:

```text id="pxldrq"
ONE_SHOT
```

ONE_SHOT chỉ tồn tại:

* đúng 1 scan cycle

---

### ✅ Ý nghĩa

Dù người vận hành giữ nút:

* hệ thống chỉ xử lý 1 lần nhấn duy nhất

---

## 3️⃣ Toggle trạng thái đèn

Mỗi khi:

```text id="gqg5ut"
ONE_SHOT = 1
```

PLC sẽ đảo trạng thái:

```text id="rn4br6"
M0.0
```

---

### Logic Toggle

| Trạng thái cũ | Trạng thái mới |
| ------------- | -------------- |
| 0             | 1              |
| 1             | 0              |

---

## 4️⃣ Output Follow State

Ngõ ra thực tế:

```text id="7vj1zw"
Q0.0
```

luôn bám theo:

```text id="0ukj5o"
M0.0
```

---

### ✅ Ý nghĩa

Tách:

* Logic State
* Physical Output

giúp chương trình:

* dễ mở rộng
* dễ debug
* đúng chuẩn công nghiệp

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Tên biến     | Địa chỉ | Kiểu | Ghi chú           |
| ------------ | ------- | ---- | ----------------- |
| PB_LOCAL     | I0.0    | BOOL | Nút nhấn          |
| DEBOUNCE_TON | T33     | TON  | Debounce 30–50 ms |
| PB_DEBOUNCED | (=T33)  | BOOL | Tín hiệu đã lọc   |
| ONE_SHOT     | M0.5    | BOOL | Xung 1 scan       |
| PB_PREV      | M1.0    | BOOL | Trạng thái trước  |
| PREV_STATE   | M1.1    | BOOL | Ảnh chụp M0.0     |
| LAMP_STATE   | M0.0    | BOOL | Trạng thái đèn    |
| LAMP_OUT     | Q0.0    | BOOL | Đèn output        |

---

# 🪜 Ladder Logic

<img width="878" height="516" alt="image" src="https://github.com/user-attachments/assets/62f35e65-028f-4e0a-bd98-2e0642ba0b15" />

---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng logic rất phổ biến trong:

* Toggle Control
* Debounce Filtering
* Push Button Interface
* Human Machine Interaction
* Operator Panel Control

---

# 📂 File dự án

```text id="g6z5ko"
Vol1_Proj05_LAD_05_toggle_debounce.mwp
```

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Thiết kế debounce logic
* ✅ Tạo ONE-SHOT chuẩn
* ✅ Toggle bằng M-bit
* ✅ Chống rung tiếp điểm
* ✅ Viết edge detection
* ✅ Thiết kế nút nhấn công nghiệp ổn định

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Double-click detection
* Long-press detection
* Toggle nhiều output
* HMI push button filtering
* Encoder pulse filtering
* Multi-button debounce

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
