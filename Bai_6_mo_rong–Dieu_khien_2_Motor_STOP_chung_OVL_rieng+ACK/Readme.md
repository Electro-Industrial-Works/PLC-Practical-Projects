⚡ Bài 6 mở rộng – Điều khiển 2 động cơ, STOP chung, OL riêng + ACK

PLC Practical Projects Series
Siemens S7-200 PLC Ladder Logic

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/85b0e560-00a3-4f21-b074-889c25343ec7" />

📘 Giới thiệu (Overview)

Bài 6 mở rộng xử lý một tình huống điều khiển công nghiệp thực tế hơn:

Hai động cơ hoạt động độc lập
Mỗi động cơ có:
START riêng
Overload riêng
Dùng chung:
STOP an toàn
Alarm tổng
Còi cảnh báo

Ngoài chức năng chạy/dừng motor, hệ thống còn bổ sung:

🚨 Đèn ALARM chung khi bất kỳ Overload nào tác động
🔊 Còi HORN chỉ kêu khi lỗi mới xuất hiện
✅ Nút ACK để tắt còi nhưng vẫn giữ Alarm

Đây là cấu trúc logic rất phổ biến trong:

Hệ thống bơm đôi
Quạt đôi
Băng tải đôi
Motor backup
Hệ thống nhiều nhánh có alarm chung
🏭 Ứng dụng thực tế

Ví dụ trong công nghiệp:

Hệ thống 2 bơm nước
Twin conveyor system
Exhaust fan system
Cooling tower dual fan
Compressor lead/lag
Multi-motor MCC panel
🎯 Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

✅ Điều khiển 2 động cơ độc lập
✅ Dùng STOP chung cho nhiều nhánh
✅ Tạo Overload riêng cho từng motor
✅ Gom nhiều lỗi thành ALARM tổng
✅ Tạo còi HORN chỉ kêu khi lỗi mới
✅ Dùng ACK để silence còi
✅ Tách M-bit khỏi Q output
✅ Tổ chức Ladder Logic đa nhánh rõ ràng
🧠 Mô tả bài toán (Problem Description)

Hệ thống gồm:

2 động cơ
1 STOP chung
2 Overload riêng
1 Alarm Lamp
1 Horn
1 ACK button
⚙ Chức năng điều khiển Motor 1
Điều kiện	Kết quả
Nhấn START_M1	Motor 1 chạy
STOP_ALL mở	Motor 1 dừng
OL_M1 mở	Motor 1 dừng

Motor 1 sử dụng:

M-bit latch

để giữ trạng thái RUN.

⚙ Chức năng điều khiển Motor 2
Điều kiện	Kết quả
Nhấn START_M2	Motor 2 chạy
STOP_ALL mở	Motor 2 dừng
OL_M2 mở	Motor 2 dừng

Motor 2 hoạt động độc lập với Motor 1.

🚨 Chức năng Alarm và Horn
Điều kiện	Kết quả
OL_M1 hoặc OL_M2 mở	AlarmActive = 1
AlarmActive xuất hiện lần đầu	HORN kêu
Nhấn ACK	HORN tắt
Lỗi vẫn còn	ALARM vẫn sáng
🔄 Nguyên lý hoạt động (Operating Principle)
1️⃣ Điều khiển hai động cơ độc lập
🔹 Motor 1

Khi:

START_M1 (I0.0)

được nhấn và:

STOP_ALL OK
OL_M1 OK

→ PLC Set:

M1_Run

→ bật:

Q0.0

Nếu:

STOP_ALL mở
hoặc OL_M1 mở

→ PLC Reset:

M1_Run

→ Motor 1 dừng ngay.

🔹 Motor 2

Motor 2 hoạt động tương tự:

START_M2 (I0.4)

→ điều khiển:

M2_Run

→ xuất ra:

Q0.1
✅ Ý nghĩa công nghiệp

Hai động cơ:

chạy độc lập
nhưng vẫn chịu:
STOP chung
hệ thống alarm chung

Đây là cấu trúc rất phổ biến trong MCC và hệ thống motor group.

2️⃣ Đèn báo lỗi chung (Alarm Lamp)

Mỗi Overload sử dụng tiếp điểm:

NC

Khi quá tải:

OL = 0

→ PLC xem như có lỗi.

Nếu:

OL_M1 = 0
OR
OL_M2 = 0

→ PLC Set:

AlarmActive

→ bật:

Q0.2
✅ Ý nghĩa

Đèn Alarm cho biết:

ít nhất một motor đang lỗi
3️⃣ Còi báo lỗi chỉ kêu khi lỗi mới xuất hiện

Đây là kỹ thuật:

Edge Detection Alarm

rất phổ biến trong công nghiệp.

⚠ Vấn đề thực tế

Nếu dùng trực tiếp:

AlarmActive

→ còi sẽ:

❌ Kêu liên tục
❌ Gây khó chịu
❌ Operator dễ bỏ qua alarm

✅ Giải pháp

PLC so sánh:

Alarm hiện tại
Alarm ở scan trước

Khi:

AlarmActive : 0 → 1

→ tạo:

NewAlarm

→ Set:

HornLatch

→ bật:

Q0.3
4️⃣ ACK – Silence Horn

Khi người vận hành nhấn:

ACK (I0.3)

→ PLC Reset:

HornLatch

→ còi tắt.

⚠ Quan trọng

ACK chỉ:

tắt còi

KHÔNG xóa lỗi.

Vì vậy:

Nếu Overload vẫn còn:

Alarm Lamp vẫn sáng
✅ Đây là chuẩn công nghiệp
Horn = thông báo lỗi mới
Alarm Lamp = duy trì trạng thái lỗi
🧩 Bảng I/O và vùng nhớ sử dụng
Ký hiệu	Địa chỉ	Kiểu	Mô tả
START_M1	I0.0	NO	Start Motor 1
STOP_ALL	I0.1	NC	STOP chung
OVL_M1	I0.2	NC	Overload Motor 1
ACK	I0.3	NO	Acknowledge Horn
START_M2	I0.4	NO	Start Motor 2
OVL_M2	I0.5	NC	Overload Motor 2
M1_Coil	Q0.0	Output	Contactor Motor 1
M2_Coil	Q0.1	Output	Contactor Motor 2
ALARM_LAMP	Q0.2	Output	Đèn Alarm
HORN	Q0.3	Output	Còi báo lỗi
AlarmActive	M0.0	Bit	Trạng thái alarm
HornLatch	M0.1	Bit	Giữ còi
PrevAlarm	M0.2	Bit	Alarm scan trước
🪜 Ladder Logic
<img width="677" height="652" alt="image" src="https://github.com/user-attachments/assets/a87f91c0-3a6b-4616-93ff-3e0845a745e8" />

🏗 Cấu trúc điều khiển công nghiệp

Bài này mô phỏng logic thường gặp trong:

MCC Panel
Multi-Motor Control
Alarm System
Fault Management
Industrial Safety Logic
📂 File dự án
Vol1_Proj06_Dual_Motor_Alarm_ACK.mwp
🛠 Kỹ năng đạt được

Sau bài này, bạn có thể:

✅ Điều khiển nhiều motor
✅ Viết logic alarm chuẩn công nghiệp
✅ Tạo horn acknowledge
✅ Viết edge detection
✅ Gom nhiều lỗi thành alarm tổng
✅ Tách logic M-bit khỏi output
🚀 Gợi ý mở rộng

Bạn có thể mở rộng thêm:

Fault history
Alarm reset
Auto restart
Lead/Lag motor
HMI alarm page
Buzzer silence timer
SCADA integration


📚 PLC Practical Projects Series

Series bao gồm:

Start/Stop
Timer
Counter
Alarm
Multi-Motor Control
Safety Logic
Industrial Automation
⭐ Nếu project hữu ích

Hãy:

⭐ Star repository
🍴 Fork project
📢 Chia sẻ cho cộng đồng PLC

để hỗ trợ phát triển thêm nhiều project PLC thực tế 🚀
