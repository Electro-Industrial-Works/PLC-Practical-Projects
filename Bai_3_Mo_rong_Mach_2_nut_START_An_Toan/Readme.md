# ⚡ Bài 3 Mở rộng – Nhấn 2 nút → Trễ 5 giây rồi LATCH, Đèn đếm ngược

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic

---

# 📘 Giới thiệu (Overview)

Bài 3 mở rộng bổ sung nhiều chức năng thực tế cho bộ định thời TON:

* ✅ Trễ bật rồi giữ trạng thái bằng LATCH
* ✅ Hiển thị trạng thái đang đếm bằng đèn nháy
* ✅ Nhập thời gian delay từ HMI thông qua vùng nhớ VW120
* ✅ Điều khiển an toàn bằng thao tác nhấn đồng thời 2 tay

Đây là cấu trúc rất phổ biến trong hệ thống công nghiệp yêu cầu:

* Chờ ổn định trước khi kích hoạt
* Countdown trước khi RUN
* Xác nhận thao tác có chủ đích
* Tránh vô tình chạm nút gây nguy hiểm
* Two-hand safety control

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Máy ép công nghiệp
* Hệ thống dao cắt
* Máy dập
* Thiết bị công suất lớn
* Băng tải cần xác nhận an toàn
* Hệ thống startup có delay

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Nhấn đồng thời 2 nút để kích hoạt hệ thống
* ✅ Tạo trễ bật bằng TON
* ✅ Set LATCH sau khi đủ thời gian
* ✅ Giữ trạng thái RUN bằng M bit
* ✅ Reset hệ thống bằng STOP
* ✅ Tạo đèn nháy countdown bằng SM0.5
* ✅ Sử dụng PT động từ HMI (VW120)
* ✅ Kết hợp Timer + Latch + Clock Bit

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống hoạt động như sau:

| Điều kiện                | Hoạt động           |
| ------------------------ | ------------------- |
| START + STOP cùng hợp lệ | Kích hoạt Timer T37 |
| Giữ đủ thời gian         | Set RUN_LATCH       |
| RUN_LATCH = 1            | Bật Q0.0            |
| Nhấn STOP                | Reset RUN_LATCH     |
| Timer đang đếm           | Đèn Q0.1 nháy       |
| Thả START sớm            | Timer reset         |

---

# ⚙ Thiết bị sử dụng

| Thiết bị          | Địa chỉ | Loại       |
| ----------------- | ------- | ---------- |
| START Push Button | I0.0    | NO         |
| STOP Push Button  | I0.1    | NC         |
| TON Timer         | T37     | TON        |
| RUN Latch         | M0.0    | Memory Bit |
| Output Device     | Q0.0    | BOOL       |
| Countdown Lamp    | Q0.1    | BOOL       |
| Clock Bit         | SM0.5   | System Bit |
| Preset Time       | VW120   | Word       |

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Điều kiện Timer bắt đầu đếm

Khi:

* START = 1
* STOP = 1

→ PLC kích hoạt Timer T37.

Preset time:

```text
PT = VW120
```

Giá trị này có thể thay đổi từ HMI.

---

## 2️⃣ Timer đếm thời gian

Timer TON bắt đầu đếm:

* ET tăng dần
* Khi ET đạt PT
  → T37 = 1

---

## 3️⃣ Đủ thời gian → Set RUN Latch

Khi:

```text
T37 = 1
```

→ PLC Set:

```text
M0.0
```

RUN_LATCH sẽ:

* bật Q0.0
* giữ trạng thái RUN

---

## 4️⃣ STOP Reset hệ thống

Nếu STOP bị nhấn:

```text
I0.1 = 0
```

→ PLC Reset:

```text
M0.0
```

→ Q0.0 OFF ngay lập tức.

⚠ STOP luôn có ưu tiên cao nhất.

---

## 5️⃣ Đèn nháy báo Countdown

Trong lúc Timer đang đếm:

* IN = 1
* T37 = 0

PLC bật đèn nháy:

```text
Q0.1 = SM0.5 AND IN AND NOT T37
```

Trong đó:

| Bit hệ thống | Ý nghĩa    |
| ------------ | ---------- |
| SM0.5        | Clock 1 Hz |

✅ Đèn nháy giúp người vận hành biết:

> “Hệ thống đang đếm thời gian.”

---

## 6️⃣ Thả START trước thời gian

Nếu START bị thả trước khi Timer hoàn tất:

* Timer reset ngay
* T37 = 0
* M0.0 không được Set

→ Hệ thống không latch.

✅ Giúp tránh kích hoạt ngoài ý muốn.

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Tên biến       | Địa chỉ | Kiểu       | Ghi chú       |
| -------------- | ------- | ---------- | ------------- |
| PB_START       | I0.0    | BOOL       | Nút START     |
| PB_STOP_NC     | I0.1    | BOOL       | Nút STOP NC   |
| RUN_LATCH      | M0.0    | BOOL       | Giữ RUN       |
| T_ON_DELAY     | T37     | TON        | Delay ON      |
| PT_HMI         | VW120   | Word       | PT từ HMI     |
| DEVICE_OUT     | Q0.0    | BOOL       | Output Device |
| COUNTDOWN_LAMP | Q0.1    | BOOL       | Đèn countdown |
| CLOCK_1HZ      | SM0.5   | System Bit | Nháy 1 Hz     |

---

# 🪜 Ladder Logic


<img width="875" height="442" alt="image" src="https://github.com/user-attachments/assets/fc16eb0d-18df-4c67-bd7d-991473ba94fc" />

---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng cấu trúc rất phổ biến trong:

* Countdown before startup
* Safe startup
* Two-hand safety control
* Delayed machine activation
* Industrial startup sequencing

---

# 📂 File dự án

```text
Vol1_Proj03Ex_LAD_03_delay_on.mwp
```

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Dùng TON nâng cao
* ✅ Kết hợp Timer + Latch
* ✅ Tạo countdown lamp
* ✅ Dùng Clock Bit SM0.5
* ✅ Đọc PT từ HMI
* ✅ Viết logic startup công nghiệp

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Alarm countdown
* Multi-step startup
* Password enable
* Safety interlock
* Auto shutdown timer
* Multi-device startup sequence

---

# 📚 PLC Practical Projects Series

Series bao gồm:

* Start/Stop
* Toggle
* Timer
* Counter
* Alarm
* Motor Control
* Industrial Logic
* Multi-Motor System
* Safety Interlock

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để hỗ trợ phát triển thêm nhiều project PLC thực tế 🚀

