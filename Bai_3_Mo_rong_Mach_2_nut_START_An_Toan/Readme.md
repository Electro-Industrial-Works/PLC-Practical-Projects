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
| START-1 + START-2 hợp lệ | Kích hoạt Timer T37 |
| Giữ đủ thời gian         | Set RUN_LATCH       |
| RUN_LATCH = 1            | Bật Q0.0            |
| Nhấn STOP                | Reset RUN_LATCH     |
| Timer đang đếm           | Đèn Q0.1 nháy       |
| Thả START sớm            | Timer reset         |

---

# ⚙ Thiết bị sử dụng

| Thiết bị          | Địa chỉ | Loại       |
| ----------------- | ------- | ---------- |
| START1 PB         | I0.0    | NO         |
| START2 Push Butt  | I0.1    | NC         |
| STOP Push Button  | I0.1    | NC         |
| TON Timer         | T37     | TON        |
| RUN Latch         | M0.0    | Memory Bit |
| Output Device     | Q0.0    | BOOL       |
| Countdown Lamp    | Q0.1    | BOOL       |
| Clock Bit         | SM0.5   | System Bit |
| Preset Time       | VW120   | Word       |

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

# Nguyên lý hoạt động
# 1. Khởi động Timer

Timer TON T37 chỉ hoạt động khi:

START-1 (I0.0) = ON
START-2 (I0.1) = ON
STOP NC (I0.2) = ON

Khi đủ điều kiện:

I0.0 AND I0.1 AND I0.2

PLC bắt đầu đếm thời gian với giá trị PT lấy từ VW120.

Ví dụ:

VW120 = 50
→ PT = 5.0 giây
# 2. Kích hoạt RUN Latch

Khi Timer đếm xong:

T37 = 1

PLC sẽ:

SET M0.0

Bit M0.0 giữ trạng thái RUN cho hệ thống.

# 3. Điều khiển thiết bị

Khi:

M0.0 = 1

PLC bật:

Q0.0 = ON

Thiết bị sẽ tiếp tục chạy ngay cả khi người vận hành thả nút START.

# 4. Dừng hệ thống

Khi nhấn STOP:

I0.2 = 0

PLC:

RESET M0.0

Kết quả:

Q0.0 OFF
hệ thống dừng
# 5. Đèn Countdown

Trong lúc Timer đang đếm:

NOT T37

đèn Q0.1 sẽ nhấp nháy theo xung clock SM0.5:

Q0.1 = SM0.5 AND IN AND NOT T37

Mục đích:

báo hiệu hệ thống đang countdown
cảnh báo người vận hành
---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Tên biến       | Địa chỉ | Kiểu       | Ghi chú       |
| -------------- | ------- | ---------- | ------------- |
| PB_START-1     | I0.0    | BOOL       | Nút START1    |
| PB_START-2     | I0.0    | BOOL       | Nút START2    |
| PB_STOP_NC     | I0.1    | BOOL       | Nút STOP NC   |
| RUN_LATCH      | M0.0    | BOOL       | Giữ RUN       |
| T_ON_DELAY     | T37     | TON        | Delay ON      |
| PT_HMI         | VW120   | Word       | PT từ HMI     |
| DEVICE_OUT     | Q0.0    | BOOL       | Output Device |
| COUNTDOWN_LAMP | Q0.1    | BOOL       | Đèn countdown |
| CLOCK_1HZ      | SM0.5   | System Bit | Nháy 1 Hz     |


---

# 🪜 Ladder Logic

<img width="932" height="1080" alt="image" src="https://github.com/user-attachments/assets/5f36c1bb-d177-4982-86c3-6e78028b2075" />

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

