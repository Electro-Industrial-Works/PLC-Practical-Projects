# ⚡ Bài 4 – Nhấn nút → Sáng 5 giây rồi tự tắt (TON + SR/RS)

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic

---

# 📘 Giới thiệu (Overview)

Bài 4 hướng dẫn cách mô phỏng chức năng **Pulse Timer (TP)** bằng:

* Timer TON
* Mạch chốt giữ SR/RS (Set/Reset)
* One-Shot Trigger

Khi người vận hành nhấn nút START:

✅ Đèn sáng đúng 5 giây
✅ Sau đó tự tắt
✅ Không cần giữ nút
✅ Nhấn giữ hoặc nhấn liên tục không kéo dài thời gian xung

Đây là kỹ thuật rất phổ biến trong hệ thống công nghiệp cần:

* Chu kỳ chạy cố định
* Tín hiệu tạm thời
* Pulse output
* Delay OFF theo thời gian xác định

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Đèn hành lang
* Chuông báo
* Máy thổi khí
* Máy đóng chai
* Máy ép
* Bơm dầu định lượng
* Tín hiệu cảnh báo ngắn hạn

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Mô phỏng TP bằng TON + SR/RS
* ✅ Tạo xung ON đúng 5 giây
* ✅ Dùng ONE_SHOT để chống giữ nút
* ✅ Tách trạng thái RUN khỏi Output Q
* ✅ Điều khiển chu trình tự tắt
* ✅ Ứng dụng TON trong Pulse Control

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống hoạt động như sau:

| Điều kiện  | Hoạt động     |
| ---------- | ------------- |
| Nhấn START | Tạo ONE_SHOT  |
| ONE_SHOT   | Set RUN       |
| RUN = 1    | Bật Q0.0      |
| RUN = 1    | Kích hoạt T37 |
| T37 đủ 5s  | Reset RUN     |
| RUN = 0    | Tắt Q0.0      |

---

## ⚙ Đặc điểm hệ thống

* Nhấn giữ START không kéo dài thời gian xung
* Nhấn nhiều lần khi đang chạy không reset timer
* Không cần giữ nút START
* Không sử dụng STOP
* Chu trình tự kết thúc sau đúng 5 giây

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Khởi động bằng ONE_SHOT

Khi người vận hành nhấn START:

```text
I0.0
```

PLC sử dụng phát hiện cạnh lên để tạo:

```text
ONE_SHOT
```

ONE_SHOT chỉ tồn tại đúng:

* 1 scan cycle

---

### ✅ Ý nghĩa

Dù người vận hành giữ START:

* PLC chỉ nhận một lệnh duy nhất
* Không khởi động lại timer liên tục

---

## 2️⃣ Set trạng thái RUN

Khi ONE_SHOT xuất hiện:

```text
Set M0.0
```

RUN sẽ:

* bật Q0.0
* kích hoạt Timer TON

---

## 3️⃣ Timer TON đếm 5 giây

Khi:

```text
RUN = 1
```

→ Timer T37 bắt đầu đếm.

Preset:

```text
PT = 5s
```

---

## 4️⃣ Kết thúc chu trình

Khi Timer hoàn tất:

```text
T37 = 1
```

PLC sẽ:

```text
Reset M0.0
```

→ RUN OFF
→ Q0.0 OFF

---

## 5️⃣ Hành vi khi giữ START

Nếu giữ START liên tục:

* Không tạo thêm ONE_SHOT
* Timer không bị reset
* Chu trình vẫn kết thúc đúng 5 giây

✅ Đây là đặc tính rất quan trọng trong Pulse Control.

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Tên biến    | Địa chỉ | Kiểu | Ghi chú                    |
| ----------- | ------- | ---- | -------------------------- |
| PB_START    | I0.0    | BOOL | Nút START                  |
| PB_STOP     | I0.1    | BOOL | STOP NC *(optional)*       |
| PB_PREV     | M1.0    | BOOL | Nhớ trạng thái START trước |
| ONE_SHOT    | M0.5    | BOOL | Xung cạnh lên              |
| PULSE_STATE | M0.0    | BOOL | RUN latch                  |
| T_PULSE     | T37     | TON  | PT = 5s                    |
| LAMP_OUT    | Q0.0    | BOOL | Output lamp                |

---

# 🪜 Ladder Logic


<img width="880" height="668" alt="image" src="https://github.com/user-attachments/assets/dfe16f99-839f-498d-97db-34489e3dc645" />

---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng cấu trúc phổ biến trong:

* Pulse Timer
* Fixed Runtime Control
* Temporary Signal System
* Timed Output
* Industrial Pulse Logic

---

# 📂 File dự án

```text
Vol1_Proj04_Pulse_Timer_SR_RS.mwp
```

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Tạo Pulse Timer bằng TON
* ✅ Sử dụng SR/RS latch
* ✅ Viết One-Shot Logic
* ✅ Điều khiển Output theo thời gian cố định
* ✅ Thiết kế chu trình tự tắt
* ✅ Tách RUN state khỏi Output

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Adjustable pulse time từ HMI
* Multiple pulse outputs
* Alarm pulse
* Warning buzzer
* Auto repeat pulse
* Flashing sequence

---

# 📚 PLC Practical Projects Series

Series bao gồm:

* Start/Stop
* Timer
* Counter
* Pulse Logic
* Alarm System
* Motor Control
* Industrial Logic
* Safety Interlock

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để chia sẻ thêm nhiều project PLC thực tế 🚀
