Bài 3 – Nhấn nút → sau 5 giây mới bật (ON Delay Timer – TON)
1. Giới thiệu (Overview)

Bài 3 giới thiệu kỹ thuật định thời bằng bộ hẹn giờ TON (ON Delay Timer), dùng khi thiết bị chỉ được bật sau khi tín hiệu đầu vào duy trì đủ thời gian.

Đây là chức năng cực kỳ phổ biến trong hệ thống công nghiệp, nơi thiết bị cần:

khởi động theo thứ tự,
ổn định trước khi chạy,
hoặc tránh kích hoạt ngoài ý muốn.

Ví dụ thực tế:

Bơm dầu bôi trơn chạy trước → sau vài giây mới cho motor chính chạy.
Quạt gió chạy trước khi cấp nhiệt.
Delay startup trong hệ thống băng tải hoặc HVAC.
2. Mục tiêu bài học (Learning Objectives)

Sau bài này, bạn sẽ hiểu cách:

✅ Sử dụng Timer TON trong PLC S7-200
✅ Tạo logic trễ bật (ON Delay)
✅ Reset timer khi nhả nút hoặc STOP tác động
✅ Tổ chức Ladder bằng nhiều Network rõ ràng
✅ Hiểu cơ chế hoạt động:

IN
PT
ET
Q

✅ Ứng dụng Timer trong hệ thống startup tuần tự công nghiệp

3. Mô tả bài toán (Problem Description)

Hệ thống sử dụng:

1 nút START
1 nút STOP
1 Timer TON 5 giây
1 ngõ ra đèn/motor

Nguyên tắc hoạt động:

Người vận hành phải giữ START liên tục đủ 5 giây.
Sau khi Timer hoàn tất:
→ PLC mới bật Q0.0.
Nếu thả START trước 5 giây:
→ Timer reset ngay.
→ Q0.0 không được bật.
STOP luôn có ưu tiên cao nhất:
→ tắt hệ thống ngay lập tức.
4. Thiết bị sử dụng
Thiết bị	Địa chỉ	Loại
START Push Button	I0.0	NO
STOP Push Button	I0.1	NC
TON Timer	T37	TON
Output Lamp / Motor	Q0.0	BOOL
5. Nguyên lý hoạt động (Operating Principle)
5.1 Điều kiện kích hoạt Timer

Khi:

START = 1
STOP = 1

→ PLC kích hoạt Timer TON T37.

5.2 Timer đếm 5 giây
T37 bắt đầu đếm.
PT = 5s.
ET tăng dần tới PT.
5.3 Đủ thời gian → bật ngõ ra

Nếu START được giữ đủ 5 giây:

→ T37.Q = 1
→ PLC bật Q0.0.

5.4 Nhả START trước thời gian

Nếu người vận hành:

nhả START trước 5 giây

→ Timer reset ngay
→ ET trở về 0
→ Q0.0 không bật.

Điều này giúp tránh kích hoạt ngoài ý muốn.

5.5 STOP ưu tiên cao nhất

Nếu STOP bị nhấn:

→ PLC reset Timer
→ Tắt Q0.0 ngay lập tức.

Đây là nguyên tắc an toàn cơ bản trong hệ thống công nghiệp.

6. Bảng I/O và vùng nhớ sử dụng
Tên biến	Địa chỉ	Kiểu	Ghi chú
PB_START	I0.0	BOOL	Nút Start
PB_STOP_NC	I0.1	BOOL	Nút Stop NC
DEBOUNCE_30ms (optional)	T32	TON	Lọc dội nút
T_ON_5S	T37	TON	Trễ bật 5 giây
LAMP_OUT	Q0.0	BOOL	Đèn / Motor Output
7. Logic Ladder
Network 1 – Timer TON
START và STOP điều khiển Timer T37.
PT = 5s.
Network 2 – Output Control
Khi T37.Q = 1
→ bật Q0.0.
8. Ứng dụng thực tế

Kỹ thuật TON được sử dụng rất nhiều trong:

Startup tuần tự
Delay motor
Delay quạt
Delay heater
HVAC
Conveyor system
Pump sequencing
9. File dự án
Vol1_Proj03_LAD_01_ON_DELAY.mwp
10. Kỹ năng đạt được

Sau bài này, bạn có thể:

✅ Thiết kế logic delay startup
✅ Sử dụng Timer TON đúng chuẩn
✅ Viết Ladder có cấu trúc rõ ràng
✅ Hiểu timer trong PLC công nghiệp
✅ Ứng dụng delay trong hệ thống thực tế
