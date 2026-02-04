# Bài 2 – Bật/Tắt thiết bị bằng 1 nút (Toggle Control)

## 1 Giới thiệu (Overview)

Bài 2 giới thiệu kỹ thuật bật/tắt thiết bị bằng **một nút duy nhất** (*toggle control*). Đây là kiểu điều khiển cực kỳ phổ biến trong thực tế: bật/tắt đèn, relay, motor nhỏ, hoặc các chức năng ON/OFF đơn giản trên bảng điều khiển.

Điểm quan trọng của bài này là: nếu chỉ dùng trực tiếp trạng thái nút nhấn, khi người vận hành giữ nút lâu, ngõ ra có thể bị **đảo liên tục** theo chu kỳ quét của PLC. Vì vậy, bài này sử dụng:

- Phát hiện cạnh (*one-shot*)
- Bit nhớ trạng thái  

để đảm bảo: **mỗi lần nhấn chỉ được tính một lần**.

Đây là kỹ thuật nền tảng khi xử lý nút nhấn trong PLC vì giúp chương trình:
- Ổn định  
- Dễ mở rộng  
- Dễ kiểm thử  

---

## 2 Mục tiêu (Learning Objectives)

Sau khi hoàn thành bài này, người học có thể:

- Điều khiển ON/OFF bằng một nút duy nhất (I0.0)  
- Sử dụng **one-shot** để phát hiện cạnh nhấn (rising edge)  
- Lưu trạng thái đèn vào bit nhớ trung gian (M) thay vì ghi trực tiếp ra Q  
- Đảm bảo giữ nút không làm đèn đảo trạng thái liên tục  
- Tách logic trạng thái (M0.0) và ngõ ra thực (Q0.0)

---

## 3 Mô tả bài toán (Problem Description)

Hệ thống chỉ có một nút nhấn dùng để điều khiển bật/tắt đèn.  
Mỗi lần nhấn, đèn phải đổi trạng thái:

- Đèn đang **tắt → bật**
- Đèn đang **bật → tắt**

Trạng thái đèn được lưu vào bit nhớ trung gian **M0.0**, và chỉ xuất ra **Q0.0** sau khi xử lý logic.

Để tránh việc giữ nút gây nhấp nháy, PLC sử dụng kỹ thuật **one-shot (xung 1 scan)** dựa trên việc ghi nhớ trạng thái cũ của nút (**M1.0**).

---

## 4 Phân tích bài toán & hình thành chương trình LAD

### Bước 1: Mỗi lần nhấn → đổi trạng thái

Đây không phải điều khiển giữ nút, mà là:

> Nhấn một lần → đổi trạng thái một lần

---

### Bước 2: Tạo tín hiệu nhấn hợp lệ bằng phát hiện cạnh (One-shot)

PLC quét liên tục. Nếu người vận hành giữ nút, I0.0 sẽ giữ mức 1 trong nhiều scan.

Nếu dùng trực tiếp I0.0 → đèn có thể đảo liên tục.

Giải pháp:
- Phát hiện cạnh lên (0 → 1)
- Tạo xung tồn tại đúng 1 scan (**ONE_SHOT**)

---

### Bước 3: Dùng bit nhớ giữ trạng thái

Đèn phải nhớ trạng thái hiện tại.

Dùng bit nhớ:


Tách khỏi Q giúp:
- Dễ kiểm tra logic
- Dễ mở rộng (Auto/Manual, khóa an toàn…)

---

### Bước 4: Toggle trạng thái và xuất ra Q

Khi có ONE_SHOT:
- Đảo M0.0 (0 ↔ 1)

Sau đó:
Q0.0 = M0.0

---

## 5 Nguyên lý hoạt động (Operating Principle)

### 1️⃣ Phát hiện cạnh lên để tạo ONE-SHOT

Khi I0.0 chuyển từ 0 → 1 → PLC tạo xung **ONE_SHOT (M0.5)** tồn tại 1 chu kỳ quét.

---

### 2️⃣ Toggle trạng thái đèn

Khi ONE_SHOT = 1 → PLC đảo M0.0.

---

### 3️⃣ Xuất ra ngõ ra thực

Q0.0 = M0.0
---

### 4️⃣ Ghi nhớ trạng thái nút ở chu kỳ trước

PLC dùng **M1.0** để lưu trạng thái I0.0 của scan trước → phục vụ phát hiện cạnh.

---

### 5️⃣ Giữ nút không làm đèn đổi liên tục

Giữ nút = không có xung mới → đèn không nhấp nháy.

---

## 6 Bảng I/O và vùng nhớ

| Tên | Địa chỉ | Kiểu | Ghi chú |
|------|---------|------|--------|
| PB_LOCAL | I0.0 | BOOL | Nút bấm (NO) |
| LAMP_STATE | M0.0 | BOOL | Trạng thái đèn |
| ONE_SHOT | M0.5 | BOOL | Xung 1 scan |
| PB_PREV | M1.0 | BOOL | Trạng thái nút chu kỳ trước |
| LAMP | Q0.0 | BOOL | Đèn ra |

---

⚠ Trong quá trình thực hành, có thể bổ sung biến nhớ nếu cần để hoàn thiện logic điều khiển.
