# ⚡ Bài 1 – Điều khiển chạy/dừng động cơ với liên động an toàn

> PLC Practical Projects Series
> Siemens S7-200 PLC Ladder Logic

---

# 📘 Giới thiệu (Overview)

Bài 1 là bài nền tảng quan trọng nhất trong lập trình PLC công nghiệp:

* Điều khiển chạy/dừng động cơ
* Mạch tự giữ (Self-hold)
* Ưu tiên STOP
* Liên động an toàn
* Emergency Stop
* Bảo vệ quá tải động cơ

Đây là cấu trúc xuất hiện gần như trong mọi hệ thống điều khiển công nghiệp thực tế:

* Motor
* Bơm
* Quạt
* Băng tải
* Máy ép
* Hệ thống HVAC

Mặc dù logic đơn giản, nhưng nếu thiết kế sai:

❌ Máy có thể tự chạy lại
❌ STOP không ưu tiên
❌ E-STOP không an toàn
❌ Quá tải không cắt động cơ

→ gây nguy hiểm cho người vận hành và thiết bị.

---

# 🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

* Điều khiển motor băng tải
* Hệ thống bơm nước
* Quạt hút công nghiệp
* Máy nén khí
* Motor mixer
* Tủ điều khiển động cơ

---

# 🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

* ✅ Điều khiển Start/Stop cơ bản
* ✅ Tạo mạch tự giữ (Self-holding)
* ✅ Thiết kế STOP ưu tiên
* ✅ Thêm Emergency Stop
* ✅ Thêm bảo vệ quá tải động cơ
* ✅ Viết Ladder Logic theo chuẩn công nghiệp
* ✅ Tách điều kiện an toàn khỏi điều kiện RUN

---

# 🧠 Mô tả bài toán (Problem Description)

Hệ thống điều khiển gồm:

| Thiết bị  | Chức năng          |
| --------- | ------------------ |
| START     | Yêu cầu chạy motor |
| STOP      | Dừng motor         |
| E-STOP    | Dừng khẩn cấp      |
| OVERLOAD  | Bảo vệ quá tải     |
| CONTACTOR | Đóng/ngắt motor    |

---

# ⚙ Yêu cầu hoạt động

| Điều kiện                 | Kết quả             |
| ------------------------- | ------------------- |
| Nhấn START                | Motor chạy          |
| Nhấn STOP                 | Motor dừng ngay     |
| E-STOP bị nhấn            | Motor dừng ngay     |
| OVERLOAD tác động         | Motor dừng ngay     |
| Điều kiện an toàn chưa OK | Không cho phép chạy |

---

# 🔄 Nguyên lý hoạt động (Operating Principle)

## 1️⃣ Khởi động động cơ

Khi người vận hành nhấn:

```text id="g7u2j1"
START (I0.0)
```

và tất cả điều kiện an toàn đều OK:

* STOP = 1
* E-STOP = 1
* OVERLOAD = 1

→ PLC bật:

```text id="f4m2qs"
Q0.0
```

→ contactor đóng
→ motor chạy.

---

## 2️⃣ Mạch tự giữ (Self-Hold)

Sau khi motor chạy:

```text id="k1j9qv"
Q0.0
```

sẽ tự giữ trạng thái RUN.

Người vận hành có thể:

* thả nút START

nhưng motor vẫn tiếp tục chạy.

---

### ✅ Ý nghĩa

Đây là nguyên tắc cơ bản trong:

* Motor starter
* Pump control
* Conveyor control
* Industrial motor circuit

---

## 3️⃣ STOP ưu tiên cao nhất

Nếu nhấn:

```text id="t6p9jw"
STOP (I0.1)
```

→ PLC ngắt:

```text id="g5x4kr"
Q0.0
```

→ motor dừng ngay lập tức.

---

### ⚠ Quy tắc công nghiệp

STOP luôn phải có ưu tiên cao hơn START.

---

## 4️⃣ Emergency Stop (E-STOP)

Nếu:

```text id="m0k2rx"
E-STOP (I0.2)
```

bị tác động:

→ PLC cắt motor ngay lập tức.

---

### ⚠ Ý nghĩa an toàn

E-STOP dùng cho:

* sự cố nguy hiểm
* bảo vệ con người
* bảo vệ thiết bị

Trong thực tế:

* E-STOP thường dùng tiếp điểm NC
* khi dây đứt → hệ thống cũng phải dừng

---

## 5️⃣ Bảo vệ quá tải động cơ

Nếu relay nhiệt hoặc cảm biến quá tải tác động:

```text id="u4z7nx"
OVERLOAD (I0.3)
```

→ PLC tắt motor ngay.

---

### ✅ Ý nghĩa

Giúp:

* bảo vệ motor
* tránh cháy cuộn dây
* tránh kẹt cơ khí
* tăng tuổi thọ thiết bị

---

# 🧩 Bảng I/O và vùng nhớ sử dụng

| Địa chỉ | Tên biến        | Kiểu | Mô tả               |
| ------- | --------------- | ---- | ------------------- |
| I0.0    | PB_START        | BOOL | Nút START           |
| I0.1    | PB_STOP_NC      | BOOL | STOP NC             |
| I0.2    | PB_ESTOP_NC     | BOOL | Emergency STOP      |
| I0.3    | OL_MOTOR_NC     | BOOL | Overload Protection |
| Q0.0    | MOTOR_CONTACTOR | BOOL | Contactor Motor     |

---

# 🪜 Cấu trúc Ladder Logic

<img width="667" height="235" alt="image" src="https://github.com/user-attachments/assets/df7cedfd-173a-403b-a838-0bfd6527b8f2" />

<img width="677" height="532" alt="image" src="https://github.com/user-attachments/assets/8399660e-86ad-4f78-849a-5270e84da5d6" />

<img width="678" height="497" alt="image" src="https://github.com/user-attachments/assets/d95924f6-204c-4058-ac32-af56de67a40c" />

<img width="677" height="747" alt="image" src="https://github.com/user-attachments/assets/d497ce2b-c44d-4c45-b55f-a83f1d517cb0" />

---

# 🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng logic rất phổ biến trong:

* Motor Starter
* MCC Panel
* Pump Control
* Conveyor System
* Industrial Motor Control
* Safety Interlock System

---

# 📂 File dự án

```text id="a8x6jw"
<img width="348" height="110" alt="image" src="https://github.com/user-attachments/assets/bfd5e7f3-dcc5-468e-90bc-01c04ec1199a" />

---

# 🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

* ✅ Viết mạch Start/Stop chuẩn
* ✅ Thiết kế Self-hold
* ✅ Thiết kế STOP ưu tiên
* ✅ Thêm E-STOP
* ✅ Thêm Overload Protection
* ✅ Viết Ladder Logic công nghiệp cơ bản

---

# 🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

* Alarm khi quá tải
* Delay startup
* Auto restart
* Multi-motor interlock
* Fault reset
* HMI status monitoring

---


# 📚 PLC Practical Projects Series

Series bao gồm:

* Start/Stop
* Timer
* Counter
* Toggle Logic
* Alarm System
* Safety Interlock
* Multi-Motor Control
* Industrial Automation

---

# ⭐ Nếu project hữu ích

Hãy:

* ⭐ Star repository
* 🍴 Fork project
* 📢 Chia sẻ cho cộng đồng PLC

để hỗ trợ phát triển thêm nhiều project PLC thực tế 🚀
