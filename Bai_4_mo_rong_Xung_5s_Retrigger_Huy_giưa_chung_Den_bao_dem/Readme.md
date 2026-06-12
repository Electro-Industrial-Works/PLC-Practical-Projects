# ⚡ Bài 4 Mở rộng – Xung 5 giây có Retrigger + Hủy giữa chừng + Đèn báo đếm

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/df236baf-4511-48c5-8cbf-4ecd10bdef24" />

---

# 📘 Giới thiệu (Overview)

Bài 4 mở rộng bổ sung các chức năng nâng cao cho mạch tạo xung 5 giây:

* ✅ Retrigger (gia hạn thời gian khi nhấn lại START)
* ✅ Hủy xung giữa chừng bằng STOP / E-STOP
* ✅ Đèn báo đếm nháy bằng SM0.5
* ✅ Reset ưu tiên cho hệ thống an toàn
* ✅ Kết hợp TON + Latch + One-Shot

Đây là dạng logic rất phổ biến trong các hệ thống công nghiệp như:

* Cảnh báo tạm thời
* Purge Fan
* Delay shutdown
* Warning lamp
* Countdown system
* Safety timeout control

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Quạt purge sau khi shutdown
* Đèn cảnh báo trước khi máy chạy
* Chuông cảnh báo thời gian chờ
* Hệ thống giữ tín hiệu tạm thời
* Delay relay
* Safety warning system

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Tạo xung thời gian cố định bằng TON
* ✅ Retrigger timer khi nhấn START lại
* ✅ Hủy xung bằng STOP / E-STOP
* ✅ Tạo countdown lamp bằng SM0.5
* ✅ Quản lý Reset ưu tiên
* ✅ Thiết kế timer công nghiệp nâng cao

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống hoạt động theo 3 chức năng chính:

| Chức năng      | Hoạt động                       |
| -------------- | ------------------------------- |
| Retrigger      | Nhấn lại START → đếm lại từ đầu |
| Cancel         | STOP/E-STOP → hủy ngay          |
| Countdown Lamp | Đèn nháy khi timer đang đếm     |

---

# ⚙ Chức năng 1 – Retrigger

Khi nhấn:

```text id="ewmjlwm"
START (I0.0)
```

→ Timer TON bắt đầu đếm 5 giây.

Nếu trong lúc đang đếm:

* người vận hành nhấn START lần nữa

→ PLC reset Timer
→ đếm lại từ đầu

✅ Xung được kéo dài thêm 5 giây tính từ lần nhấn cuối.

---

# ⚙ Chức năng 2 – Hủy giữa chừng (Cancel)

Nếu:

```text id="w8gdnh"
STOP (I0.1)
```

hoặc:

```text id="lnlf0r"
E-STOP (I0.2)
```

bị ngắt:

→ PLC reset toàn bộ hệ thống ngay lập tức:

* Timer OFF
* Q0.0 OFF
* Q0.1 OFF
* Hủy trạng thái RUN

⚠ Đây là yêu cầu bắt buộc trong hệ thống công nghiệp an toàn.

---

# ⚙ Chức năng 3 – Countdown Lamp

Trong lúc Timer đang đếm:

```text id="v8d8db"
IN = 1 AND T37.Q = 0
```

→ Đèn:

```text id="sl1ml2"
Q0.1
```

nháy theo:

```text id="ns4uzr"
SM0.5
```

✅ Giúp người vận hành biết:

> “Hệ thống đang đếm thời gian.”

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Khởi động bằng ONE_SHOT

Khi START được nhấn:

```text id="f72f0j"
I0.0
```

PLC tạo:

```text id="26k6ui"
ONE_SHOT
```

ONE_SHOT chỉ tồn tại đúng:

* 1 scan cycle

---

## 2️⃣ Timer bắt đầu đếm

Ngay khi ONE_SHOT xuất hiện:

* PLC reset T37
* Timer bắt đầu đếm 5 giây

Preset:

```text id="hsh3tb"
PT = 5s
```

---

## 3️⃣ Retrigger Timer

Trong khi Timer đang chạy:

Nếu START được nhấn lại:

→ PLC tạo retrigger edge

→ reset T37

→ Timer đếm lại từ đầu

✅ Đây là cơ chế:

> “Refresh timeout”

rất phổ biến trong hệ thống cảnh báo.

---

## 4️⃣ Dừng khẩn cấp & Reset ưu tiên

Nếu:

* STOP mở
* E-STOP mở

PLC sẽ:

* reset Timer
* reset trạng thái RUN
* tắt toàn bộ output

⚠ Safety luôn có ưu tiên cao nhất.

---

## 5️⃣ Hành vi Output

### 🔹 Trong lúc Timer đang đếm

```text id="pb4z2j"
Q0.0 = ON
```

### 🔹 Khi Timer hoàn tất

```text id="zq1m55"
T37 = 1
```

→ PLC tắt:

```text id="quu5f9"
Q0.0
```

---

# 💡 Countdown Lamp Logic

Trong lúc Timer đang đếm:

```text id="p2xutk"
Q0.1 = SM0.5 AND IN AND NOT T37
```

Trong đó:

| Bit hệ thống | Ý nghĩa    |
| ------------ | ---------- |
| SM0.5        | Clock 1 Hz |

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Tên biến       | Địa chỉ | Kiểu | Ghi chú        |
| -------------- | ------- | ---- | -------------- |
| PB_START       | I0.0    | BOOL | Nút START      |
| PB_STOP_NC     | I0.1    | BOOL | STOP an toàn   |
| PB_ESTOP_NC    | I0.2    | BOOL | Emergency STOP |
| ONE_SHOT       | M1.1    | BOOL | Xung cạnh lên  |
| RETRIG_EDGE    | M1.2    | BOOL | Xung retrigger |
| RETRIG_REQ     | M1.3    | BOOL | Cờ retrigger   |
| PULSE_STATE    | M0.0    | BOOL | Trạng thái RUN |
| T_PULSE        | T37     | TON  | Delay 5 giây   |
| LAMP_OUT       | Q0.0    | BOOL | Đèn chính      |
| COUNTDOWN_LAMP | Q0.1    | BOOL | Đèn countdown  |
| CLOCK_1HZ      | SM0.5   | BOOL | Clock 1 Hz     |

---

# 🪜 Ladder Logic
<img width="807" height="767" alt="image" src="https://github.com/user-attachments/assets/76b5b5b7-be13-439b-81a9-d8f25149c7c1" />

---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng logic rất phổ biến trong:

* Timeout system
* Purge fan
* Safety warning
* Alarm retrigger
* Delay shutdown
* Temporary signal control

---

# 📂 File dự án

```text id="k20z8y"
Vol1_Proj04Ex_LAD_04_pulse_5s_SR_1_shot.mwp
```

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Thiết kế Retrigger Timer
* ✅ Dùng TON nâng cao
* ✅ Viết Safety Reset Logic
* ✅ Thiết kế Countdown Lamp
* ✅ Tạo timeout system
* ✅ Kết hợp One-Shot + TON + Reset

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Adjustable PT từ HMI
* Alarm buzzer
* Multi-stage warning
* Flashing sequence
* Auto retry system
* Multi-output timeout


---

# 📚 PLC Practical Projects Series

Series bao gồm:

* Start/Stop
* Timer
* Counter
* Pulse Logic
* Alarm System
* Safety Logic
* Industrial Automation
* Multi-Motor Control

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để hỗ trợ phát triển thêm nhiều project PLC thực tế 🚀
