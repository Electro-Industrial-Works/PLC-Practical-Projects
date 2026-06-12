# ⚡ Bài 6 mở rộng – Điều khiển 2 động cơ, STOP chung, OL riêng + ACK

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f8aad866-14c1-4e2e-adcc-0cbb54be189b" />

---

# 📘 Giới thiệu (Overview)

Bài 6 mở rộng xử lý tình huống công nghiệp thực tế với:

* 2 động cơ hoạt động độc lập
* START riêng cho từng motor
* Overload riêng cho từng nhánh
* STOP chung cho toàn hệ thống
* Alarm tổng
* Horn cảnh báo + ACK

Ngoài điều khiển chạy/dừng motor, bài này còn bổ sung:

* 🚨 Đèn ALARM chung khi bất kỳ OL nào tác động
* 🔊 Còi HORN chỉ kêu khi lỗi mới phát sinh
* ✅ ACK để tắt còi nhưng vẫn giữ trạng thái Alarm

Mỗi động cơ sử dụng:

```text id="u3k7qv"
M-bit latch
```

để giữ trạng thái RUN, không Set/Reset trực tiếp trên output Q.

Đây là cấu trúc logic rất phổ biến trong:

* Hệ thống bơm đôi
* Băng tải kép
* Quạt kép
* MCC panel
* Multi-motor control system

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Hệ thống 2 bơm luân phiên
* Quạt hút đôi
* Twin conveyor
* Compressor lead/lag
* Hệ thống HVAC nhiều motor
* Tủ điều khiển nhiều động cơ

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Điều khiển 2 motor độc lập
* ✅ Dùng STOP chung cho nhiều nhánh
* ✅ Tạo Overload riêng cho từng motor
* ✅ Gom nhiều lỗi thành Alarm chung
* ✅ Tạo Horn chỉ kêu khi lỗi mới
* ✅ ACK để silence Horn
* ✅ Tách M-bit khỏi Q output
* ✅ Viết Ladder Logic đa nhánh rõ ràng

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống gồm:

* 2 động cơ
* 1 STOP chung
* 2 Overload riêng
* 1 Alarm Lamp
* 1 Horn
* 1 ACK button

---

# ⚙ Chức năng Motor 1

| Điều kiện     | Kết quả      |
| ------------- | ------------ |
| Nhấn START_M1 | Motor 1 chạy |
| STOP_ALL mở   | Motor 1 dừng |
| OL_M1 mở      | Motor 1 dừng |

Motor 1 sử dụng:

```text id="v6m2pw"
M1_Run
```

để latch trạng thái RUN.

---

# ⚙ Chức năng Motor 2

| Điều kiện     | Kết quả      |
| ------------- | ------------ |
| Nhấn START_M2 | Motor 2 chạy |
| STOP_ALL mở   | Motor 2 dừng |
| OL_M2 mở      | Motor 2 dừng |

Motor 2 hoạt động hoàn toàn độc lập với Motor 1.

---

# 🚨 Chức năng Alarm & Horn

| Điều kiện           | Kết quả         |
| ------------------- | --------------- |
| OL_M1 hoặc OL_M2 mở | AlarmActive = 1 |
| Alarm mới xuất hiện | Horn ON         |
| Nhấn ACK            | Horn OFF        |
| Lỗi vẫn tồn tại     | Alarm vẫn sáng  |

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Điều khiển hai động cơ độc lập

### 🔹 Motor 1

Khi:

```text id="a8f4rx"
START_M1 (I0.0)
```

được nhấn và:

* STOP_ALL OK
* OL_M1 OK

→ PLC Set:

```text id="t5q9mv"
M1_Run
```

→ bật:

```text id="y1k7zp"
Q0.0
```

---

Nếu:

* STOP_ALL mở
* hoặc OL_M1 mở

→ PLC Reset:

```text id="g2u6kn"
M1_Run
```

→ Motor 1 dừng ngay.

---

### 🔹 Motor 2

Motor 2 hoạt động tương tự:

```text id="m4w8qs"
START_M2 (I0.4)
```

→ điều khiển:

```text id="r7p3yx"
M2_Run
```

→ xuất ra:

```text id="f9n5kv"
Q0.1
```

---

### ✅ Ý nghĩa công nghiệp

Hai motor:

* chạy độc lập
* nhưng dùng chung:

  * STOP
  * Alarm system

Đây là cấu trúc rất phổ biến trong MCC Panel và hệ thống motor group.

---

## 2️⃣ Alarm Lamp – Đèn báo lỗi chung

Mỗi Overload sử dụng tiếp điểm:

```text id="n8x4pm"
NC
```

Khi quá tải:

```text id="w6t1jr"
OL = 0
```

→ PLC xem như có lỗi.

---

Nếu:

```text id="b3k7zu"
OL_M1 = 0
OR
OL_M2 = 0
```

→ PLC Set:

```text id="d5q2vx"
AlarmActive
```

→ bật:

```text id="u9m4rf"
Q0.2
```

---

### ✅ Ý nghĩa

Alarm Lamp cho biết:

* ít nhất một nhánh đang lỗi

---

## 3️⃣ Horn chỉ kêu khi lỗi mới xuất hiện

Đây là kỹ thuật:

```text id="h7v1qy"
Alarm Edge Detection
```

rất phổ biến trong công nghiệp.

---

### ⚠ Vấn đề thực tế

Nếu dùng trực tiếp:

```text id="p6n3kw"
AlarmActive
```

→ Horn sẽ:

❌ Kêu liên tục
❌ Gây khó chịu
❌ Operator dễ bỏ qua alarm

---

### ✅ Giải pháp

PLC so sánh:

* Alarm hiện tại
* Alarm ở scan trước

---

Khi:

```text id="e2r8tx"
AlarmActive : 0 → 1
```

→ tạo:

```text id="x5u9mk"
NewAlarm
```

→ Set:

```text id="q4v6pz"
HornLatch
```

→ bật:

```text id="s8m1ry"
Q0.3
```

---

## 4️⃣ ACK – Silence Horn

Khi người vận hành nhấn:

```text id="k3w7qn"
ACK (I0.3)
```

→ PLC Reset:

```text id="z1p8vu"
HornLatch
```

→ Horn OFF.

---

### ⚠ Quan trọng

ACK chỉ:

* tắt còi

KHÔNG xóa lỗi.

---

### Vì vậy:

Nếu Overload vẫn còn:

```text id="m5u2rx"
Alarm Lamp vẫn sáng
```

---

### ✅ Đây là chuẩn công nghiệp

* Horn = thông báo lỗi mới
* Alarm Lamp = duy trì trạng thái lỗi

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Ký hiệu     | Địa chỉ | Kiểu   | Mô tả                   |
| ----------- | ------- | ------ | ----------------------- |
| START_M1    | I0.0    | NO     | Nút Start Motor 1       |
| STOP_ALL    | I0.1    | NC     | STOP chung              |
| OVL_M1      | I0.2    | NC     | Overload Motor 1        |
| ACK         | I0.3    | NO     | Nút ACK tắt Horn        |
| START_M2    | I0.4    | NO     | Nút Start Motor 2       |
| OVL_M2      | I0.5    | NC     | Overload Motor 2        |
| M1_Coil     | Q0.0    | Output | Coil điều khiển Motor 1 |
| M2_Coil     | Q0.1    | Output | Coil điều khiển Motor 2 |
| ALARM_LAMP  | Q0.2    | Output | Đèn Alarm               |
| HORN        | Q0.3    | Output | Còi báo lỗi             |
| AlarmActive | M0.0    | Bit    | Trạng thái Alarm        |
| HornLatch   | M0.1    | Bit    | Giữ trạng thái Horn     |
| PrevAlarm   | M0.2    | Bit    | Alarm scan trước        |

---

# 🪜 Cấu trúc Ladder Logic

<img width="667" height="642" alt="image" src="https://github.com/user-attachments/assets/fb529b37-97de-4ca7-a4a0-ce192b322bd4" />

---

## 🔹 Network 6 – Output Control

| Output | Chức năng  |
| ------ | ---------- |
| Q0.0   | Motor 1    |
| Q0.1   | Motor 2    |
| Q0.2   | Alarm Lamp |
| Q0.3   | Horn       |

---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng logic thường gặp trong:

* MCC Panel
* Multi-Motor Control
* Alarm System
* Fault Management
* Industrial Safety Logic

---

# 📂 File dự án

```text id="r6v2pk"
Vol1_Proj06_Dual_Motor_Alarm_ACK.mwp
```

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Điều khiển nhiều motor
* ✅ Viết logic Alarm chuẩn công nghiệp
* ✅ Tạo Horn Acknowledge
* ✅ Viết Edge Detection
* ✅ Gom nhiều lỗi thành Alarm tổng
* ✅ Tách logic M-bit khỏi Output

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Fault history
* Alarm reset
* Auto restart
* Lead/Lag motor
* HMI alarm page
* SCADA integration

---



# 📚 PLC Practical Projects Series

Series bao gồm:

* Start/Stop
* Timer
* Counter
* Alarm
* Multi-Motor Control
* Safety Logic
* Industrial Automation

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để hỗ trợ phát triển thêm nhiều project PLC thực tế 🚀
