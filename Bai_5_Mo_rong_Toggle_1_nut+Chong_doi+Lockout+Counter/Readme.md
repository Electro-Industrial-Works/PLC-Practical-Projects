# ⚡ Bài 5 Mở rộng – Toggle 1 nút + Chống dội + Lockout + Counter

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic

---

# 📘 Giới thiệu (Overview)

Bài 5 mở rộng nâng cấp mạch Toggle 1 nút bằng nhiều kỹ thuật thực tế thường gặp trong hệ thống công nghiệp:

* ✅ Debounce Filtering từ HMI
* ✅ ONE-SHOT sạch và ổn định
* ✅ Lockout chống double-click
* ✅ Counter đếm số lần bật
* ✅ Đèn RUN nháy khi đang ON

Đây là dạng mạch rất phổ biến trong:

* Tủ điều khiển operator
* HMI Panel
* Nút ON/OFF công nghiệp
* Hệ thống chống thao tác nhầm
* Human Machine Interface

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Panel điều khiển máy
* Nút nhấn operator
* Toggle điều khiển motor
* Nút RUN/STOP
* Giao diện HMI
* Hệ thống chống double-click

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Dùng TON với PT từ HMI để debounce
* ✅ Tạo ONE-SHOT ổn định
* ✅ Chống double-click bằng Lockout
* ✅ Toggle trạng thái bằng M-bit
* ✅ Đếm số lần bật bằng CTU
* ✅ Hiển thị RUN bằng đèn nháy

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống Toggle 1 nút được mở rộng với nhiều chức năng thực tế:

| Chức năng | Mô tả                |
| --------- | -------------------- |
| Debounce  | Lọc dội bằng TON     |
| ONE_SHOT  | Tạo xung sạch        |
| Lockout   | Chống nhấn quá nhanh |
| Counter   | Đếm số lần bật       |
| RUN Blink | Đèn nháy khi ON      |

---

# ⚙ Chức năng 1 – Debounce từ HMI

Giá trị debounce được nhập từ:

```text id="1mjlwm"
VW120
```

Khoảng hợp lệ:

```text id="c1k9q5"
20–100 ms
```

Tín hiệu:

```text id="0uxi6y"
I0.0
```

được đưa qua:

```text id="hr1iwg"
TON T33
```

→ tạo:

```text id="aq8szd"
PB_DB
```

---

## ✅ Ý nghĩa

Giúp loại bỏ:

* Contact Bounce
* Nhiễu tiếp điểm
* Cạnh giả

---

# ⚙ Chức năng 2 – ONE_SHOT sạch

PLC so sánh:

* PB_DB hiện tại
* PB_PREV trước đó

Khi:

```text id="6pnkzh"
PB_DB: 0 → 1
```

→ tạo:

```text id="fjlwm3"
ONE_SHOT
```

ONE_SHOT chỉ tồn tại:

* đúng 1 scan cycle

---

## ✅ Ý nghĩa

Dù người vận hành:

* nhấn nhanh
* giữ nút

→ PLC vẫn chỉ nhận:

* đúng 1 lần nhấn

---

# ⚙ Chức năng 3 – Lockout 300 ms

Sau mỗi lần Toggle:

PLC chạy:

```text id="2gq9r3"
T39
```

Preset:

```text id="8pmv2u"
300 ms
```

Trong thời gian Lockout:

❌ Các ONE_SHOT mới bị bỏ qua

---

## ✅ Ý nghĩa

Ngăn:

* Double-click
* Nhấn quá nhanh
* Toggle liên tục do rung tay

---

# ⚙ Chức năng 4 – RUN Blink

Khi:

```text id="e0h5mk"
LAMP_STATE = 1
```

→ đèn:

```text id="1odm8l"
Q0.1
```

nháy theo:

```text id="0gkv7w"
SM0.5
```

Khi OFF:

```text id="jzjlwm"
Q0.1 = 0
```

---

## ✅ Ý nghĩa

Giúp người vận hành nhận biết:

> “Hệ thống đang ON”

một cách trực quan hơn.

---

# ⚙ Chức năng 5 – Counter đếm số lần bật

Mỗi lần:

```text id="q0wq4x"
OFF → ON
```

Counter:

```text id="5a2j5g"
C0
```

tăng lên 1.

---

## Reset Counter

Người vận hành nhấn:

```text id="qsljmt"
I0.1
```

→ reset Counter về 0.

---

## ✅ Ứng dụng

* Đếm số lần bật máy
* Theo dõi thao tác operator
* Thống kê vận hành
* Maintenance tracking

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Debounce Filtering

Tín hiệu:

```text id="8cq6gv"
I0.0
```

được lọc qua:

```text id="hm8b2d"
TON T33
```

Preset lấy từ:

```text id="y1z4qv"
VW120
```

→ tạo tín hiệu sạch:

```text id="1u0x0f"
PB_DB
```

---

## 2️⃣ Rising Edge Detection

PLC tạo:

```text id="zaxg3g"
ONE_SHOT
```

khi:

```text id="pjlwmj"
PB_DB: 0 → 1
```

---

## 3️⃣ Toggle State

Nếu:

```text id="j1up6v"
LOCK_ACTIVE = 0
```

→ ONE_SHOT được phép:

* đảo trạng thái M0.0

---

### Logic Toggle

| Trạng thái cũ | Trạng thái mới |
| ------------- | -------------- |
| OFF           | ON             |
| ON            | OFF            |

---

## 4️⃣ Lockout Timer

Sau mỗi lần Toggle:

```text id="a5zjlwm"
T39
```

được kích hoạt trong:

```text id="xt03yd"
300 ms
```

→ cấm Toggle mới.

---

## 5️⃣ RUN Blink

Khi:

```text id="jlwmrq"
M0.0 = 1
```

→ PLC bật:

```text id="cq9wcf"
Q0.1 = SM0.5
```

---

## 6️⃣ Counter CTU

Khi:

```text id="k5b9tr"
OFF → ON
```

→ Counter:

```text id="jlwmv4"
C0
```

tăng lên 1.

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Nhóm     | Địa chỉ | Tên ký hiệu    | Mô tả              |
| -------- | ------- | -------------- | ------------------ |
| Inputs   | I0.0    | PB_LOCAL       | Nút nhấn           |
| Inputs   | I0.1    | RESET_CNT      | Reset Counter      |
| Outputs  | Q0.0    | LAMP_OUT       | Đèn chính          |
| Outputs  | Q0.1    | RUN_BLINK      | Đèn RUN nháy       |
| Timer    | T33     | TON_DEBOUNCE   | Debounce TON       |
| Timer    | T39     | TON_LOCKOUT    | Lockout 300 ms     |
| Counter  | C0      | CTU_COUNT      | Counter đếm        |
| M Bit    | M0.5    | ONE_SHOT       | Xung 1 scan        |
| M Bit    | M1.0    | PB_PREV        | Trạng thái trước   |
| M Bit    | M1.1    | PREV_STATE     | Ảnh M0.0           |
| M Bit    | M0.0    | LAMP_STATE     | Trạng thái đèn     |
| M Bit    | M0.6    | TRIG_OK        | Toggle hợp lệ      |
| M Bit    | M2.0    | LOCK_ACTIVE    | Lockout active     |
| V Memory | VW120   | PT_DEBOUNCE_ms | PT debounce từ HMI |

---

# 🪜 Ladder Logic

<img width="717" height="811" alt="image" src="https://github.com/user-attachments/assets/ce45b97c-6db1-413a-9b03-e42e475bdf97" />

---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng logic phổ biến trong:

* Operator Panel
* Toggle Interface
* Human Machine Control
* Anti Double-Click Logic
* Industrial Push Button Control

---

# 📂 File dự án

```text id="jlwm3s"
Vol1_Proj05Ex_LAD_05_toggle_debounce.mwp
```

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Debounce bằng HMI
* ✅ Tạo ONE_SHOT chuẩn
* ✅ Thiết kế Lockout Logic
* ✅ Chống double-click
* ✅ Dùng Counter CTU
* ✅ Tạo RUN Blink
* ✅ Viết Toggle công nghiệp nâng cao

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Password unlock
* Long press detection
* Multi-button panel
* Alarm counter
* Maintenance statistics
* HMI runtime display

---

# 📚 PLC Practical Projects Series

Series bao gồm:

* Start/Stop
* Timer
* Counter
* Toggle Logic
* Debounce
* One-Shot
* Lockout
* Industrial Automation

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để hỗ trợ phát triển thêm nhiều project PLC thực tế 🚀
