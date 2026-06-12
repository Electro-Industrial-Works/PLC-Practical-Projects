# ⚡ Bài 3 – Nhấn nút → Sau 5 giây mới bật (ON Delay Timer – TON)

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/2fd1f929-be07-41d2-b5dd-5a21a4c1dc7a" />

---

# 📘 Giới thiệu (Overview)

Bài 3 giới thiệu kỹ thuật định thời bằng bộ hẹn giờ **TON (ON Delay Timer)**, dùng khi thiết bị chỉ được bật sau khi tín hiệu đầu vào duy trì đủ thời gian.

Đây là chức năng cực kỳ phổ biến trong hệ thống công nghiệp, nơi thiết bị cần:

✅ Khởi động theo thứ tự
✅ Chờ ổn định trước khi chạy
✅ Tránh kích hoạt ngoài ý muốn
✅ Delay startup cho motor/quạt/bơm

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Bơm dầu bôi trơn chạy trước → sau vài giây mới cho motor chính chạy
* Quạt gió chạy trước heater
* Delay startup trong hệ thống HVAC
* Khởi động tuần tự băng tải
* Delay kích hoạt relay công suất lớn

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Sử dụng Timer TON trong PLC S7-200
* ✅ Tạo logic trễ bật (ON Delay)
* ✅ Reset timer khi nhả nút
* ✅ Dừng hệ thống bằng STOP ưu tiên cao nhất
* ✅ Tổ chức Ladder Logic bằng nhiều Network rõ ràng
* ✅ Hiểu cơ chế:

  * IN
  * PT
  * ET
  * Q

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống sử dụng:

| Thiết bị | Chức năng            |
| -------- | -------------------- |
| START    | Yêu cầu bật hệ thống |
| STOP     | Dừng ngay lập tức    |
| TON T37  | Delay 5 giây         |
| Q0.0     | Đèn / Motor Output   |

---

## ⚙ Nguyên tắc hoạt động

* Người vận hành phải giữ START liên tục đủ 5 giây
* Sau khi Timer hoàn tất:
  → PLC mới bật Q0.0
* Nếu nhả START trước 5 giây:
  → Timer reset ngay
  → Q0.0 không bật
* STOP luôn có ưu tiên cao nhất

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Điều kiện kích hoạt Timer

Khi:

* START = 1
* STOP = 1

→ PLC kích hoạt Timer TON T37.

---

## 2️⃣ Timer đếm đủ 5 giây

* Timer bắt đầu đếm
* PT = 5s
* ET tăng dần tới PT

---

## 3️⃣ Đủ thời gian → bật Output

Nếu START được giữ liên tục đủ 5 giây:

```text id="x7r1cq"
T37.Q = 1
```

→ PLC bật:

```text id="eq4qaf"
Q0.0
```

---

## 4️⃣ Nhả START trước thời gian

Nếu START bị thả trước khi đủ 5 giây:

* Timer reset ngay
* ET trở về 0
* Output không bật

✅ Giúp tránh kích hoạt ngoài ý muốn.

---

## 5️⃣ STOP ưu tiên cao nhất

Nếu STOP bị nhấn:

```text id="qjlwmk"
I0.1 = 0
```

→ PLC tắt hệ thống ngay lập tức.

⚠ Đây là nguyên tắc an toàn rất quan trọng trong PLC công nghiệp.

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Tên biến                   | Địa chỉ | Kiểu | Ghi chú             |
| -------------------------- | ------- | ---- | ------------------- |
| PB_START                   | I0.0    | BOOL | Nút START           |
| PB_STOP_NC                 | I0.1    | BOOL | STOP NC             |
| DEBOUNCE_30ms *(optional)* | T32     | TON  | Lọc dội nút         |
| T_ON_5S                    | T37     | TON  | Delay ON 5 giây     |
| LAMP_OUT                   | Q0.0    | BOOL | Output Lamp / Motor |

---

# 🪜 Cấu trúc Ladder Logic

## 🔹 Network 1 – Timer TON

* START + STOP điều khiển Timer T37
* PT = 5s

---

## 🔹 Network 2 – Output Control

Khi:

```text id="50e1ji"
T37.Q = 1
```

→ bật:

```text id="teew67"
Q0.0
```

---

# 📂 File dự án

```text id="8r1vzz"
Vol1_Proj03_LAD_01_ON_DELAY.mwp
```
<img width="751" height="577" alt="image" src="https://github.com/user-attachments/assets/11453cb1-5017-46f4-9d43-e4da6a6ff355" />

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

✅ Thiết kế logic delay startup
✅ Sử dụng TON chuẩn công nghiệp
✅ Viết Ladder Logic rõ ràng
✅ Hiểu nguyên lý Timer trong PLC
✅ Ứng dụng startup tuần tự thực tế

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng bài này bằng:

* TOF (OFF Delay Timer)
* TP (Pulse Timer)
* Delay nhiều motor
* Alarm Delay
* Startup sequence
* Auto start system

---

# 📚 Series PLC Practical Projects

Repository này thuộc series thực hành PLC trong tài liệu: PLC THỰC CHIẾN -HƯỚNG DẪN THIẾT KÊ HỆ THỐNG TỰ ĐỘNG ĐIỀU KHIỂN THEO CHUẨN CÔNG NGHIỆP":

* PLC cơ bản
* Timer / Counter
* Motor Control
* Alarm System
* Industrial Logic
* Startup Sequence
* Multi-Motor Control
* 14 bài toán chuẩn công nghiệp

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để cùng nhau chia sẻ thêm nhiều bài toán thực tế trong các nhà máy  🚀
