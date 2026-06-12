# 🏗️ PLC Practical Projects – Góc nhìn từ người đi làm

![PLC](https://img.shields.io/badge/PLC-Siemens%20S7--200-blue)
![Ladder Logic](https://img.shields.io/badge/Ladder%20Logic-Industrial%20Automation-orange)
![Status](https://img.shields.io/badge/Status-Active-success)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Practical-brightgreen)
![Language](https://img.shields.io/badge/Language-Vietnamese-red)

---

## 📌 Giới thiệu

Xin chào các bạn,

Trong môi trường công nghiệp thực tế, mọi thứ không chỉ là **“chạy được”**, mà phải:

* **Chạy ổn định**
* **An toàn**
* **Dễ kiểm tra**
* **Dễ bảo trì**
* **Đạt tiêu chuẩn công nghiệp**
* **Có thể bàn giao được**

Repository này được tạo ra để chia sẻ các bài thực hành PLC theo góc nhìn thực tế ngoài hiện trường, không chỉ là lý thuyết trên lớp.

Nếu bạn đang:

* Học PLC để đi làm theo đúng thực tế công nghiệp
* Muốn nâng trình lập trình điều khiển
* Đang làm kỹ thuật nhưng muốn chắc nền tảng
* Muốn hiểu cách viết Ladder Logic rõ ràng, an toàn và dễ bảo trì

→ Bạn đang ở đúng nơi.

---

## 🎯 Triết lý khi xây dựng repository này

Chúng tôi tin rằng học PLC không nên bắt đầu từ những thứ quá phức tạp.

Bạn cần nắm chắc:

1. Bản chất điều khiển
2. Tư duy logic
3. Nguyên tắc an toàn cơ bản
4. Cách tổ chức chương trình rõ ràng
5. Cách xử lý lỗi thực tế
6. Cách viết logic theo chuẩn công nghiệp

Vì vậy, các project ở đây được thiết kế theo tiêu chí:

* ✅ Có đầy đủ logic điều khiển
* ✅ Có mô tả nguyên lý hoạt động
* ✅ Có bảng I/O rõ ràng
* ✅ Có file chương trình PLC
* ✅ Có thể thực hành lại được
* ✅ Có định hướng ứng dụng thực tế
* ✅ Không giữ lại phần quan trọng

---

## 🧭 Learning Path – Lộ trình học PLC thực chiến

```text
PLC Practical Projects
│
├── 01. Start/Stop Motor Control
├── 02. Toggle Control
├── 03. Timer TON / Delay ON
├── 04. Pulse Timer Logic
├── 05. Debounce + One-Shot
├── 06. Alarm + ACK + Multi-Motor Control
├── 07. Counter
├── 08. Interlock
├── 09. Safety Logic
├── 10. HMI Parameter
├── 11. Industrial Alarm System
├── 12. Sequential Control
├── 13. Motor Group Control
└── 14. Practical Industrial Project
```

---

## 📚 Danh sách bài học

| Bài           | Chủ đề                                              | Kỹ thuật chính                          | Trạng thái       |
| ------------- | --------------------------------------------------- | --------------------------------------- | ---------------- |
| Bài 1         | Điều khiển chạy/dừng động cơ                        | Start/Stop, Self-hold, Safety Interlock | ✅ Đã cập nhật    |
| Bài 2         | Bật/tắt máy bằng 1 nút                              | Toggle Control, One-Shot                | ✅ Đã cập nhật    |
| Bài 3         | Nhấn nút → sau 5 giây mới bật                       | TON Timer, Delay ON                     | ✅ Đã cập nhật    |
| Bài 3 mở rộng | Nhấn 2 nút, trễ 5s rồi Latch, đèn đếm ngược         | TON, Latch, HMI PT, SM0.5               | ✅ Đã cập nhật    |
| Bài 4         | Nhấn nút → sáng 5 giây rồi tắt                      | TON + SR/RS, Pulse Logic                | ✅ Đã cập nhật    |
| Bài 4 mở rộng | Xung 5s có Retrigger + Hủy giữa chừng + Đèn báo đếm | Retrigger, Cancel, Countdown Lamp       | ✅ Đã cập nhật    |
| Bài 5         | Lọc chống dội và tạo xung sạch ONE-SHOT             | Debounce, TON, Rising Edge              | ✅ Đã cập nhật    |
| Bài 5 mở rộng | Toggle 1 nút + Chống dội + Lockout + Counter        | Debounce HMI, Lockout, CTU              | ✅ Đã cập nhật    |
| Bài 6         | Điều khiển 2 động cơ, STOP chung, OL riêng + ACK    | Multi-Motor, Alarm, Horn, ACK           | 🔄 Đang cập nhật |
| Bài 7         | Counter trong PLC                                   | CTU, Reset, Count Event                 | 🔜 Sắp cập nhật  |
| Bài 8         | Liên động điều khiển                                | Interlock Logic                         | 🔜 Sắp cập nhật  |
| Bài 9         | Điều khiển tuần tự                                  | Sequence Control                        | 🔜 Sắp cập nhật  |
| Bài 10        | Alarm công nghiệp                                   | Alarm, Fault, Reset                     | 🔜 Sắp cập nhật  |
| Bài 11–14     | Project tổng hợp                                    | Industrial Control Project              | 🔜 Sắp cập nhật  |

> Nội dung từng bài sẽ tiếp tục được cập nhật theo thời gian.

---

## 📂 Cấu trúc repository đề xuất

```text
PLC-Practical-Projects
│
├── README.md
│
├── Bai_01_Start_Stop_Motor
│   ├── README.md
│   ├── *.mwp
│   └── images/
│
├── Bai_02_Toggle_Control
│   ├── README.md
│   ├── *.mwp
│   └── images/
│
├── Bai_03_ON_Delay_Timer
│   ├── README.md
│   ├── *.mwp
│   └── images/
│
├── Bai_04_Pulse_Timer
│   ├── README.md
│   ├── *.mwp
│   └── images/
│
└── Bai_05_Debounce_OneShot
    ├── README.md
    ├── *.mwp
    └── images/
```

---

## ⚙️ Phần mềm & phần cứng sử dụng

### Phần mềm

* STEP 7-Micro/WIN V4.0
* Siemens S7-200 PLC
* Ladder Logic

### Phần cứng minh họa

* PLC Siemens S7-200 CPU 224 hoặc tương đương
* Push Button NO/NC
* Đèn báo
* Relay trung gian
* Motor mô phỏng
* HMI cơ bản nếu bài có sử dụng vùng nhớ VW

---

## 🧠 Các kỹ thuật PLC có trong repository

Repository này tập trung vào các kỹ thuật thường gặp trong công nghiệp:

* Start/Stop Motor Control
* Self-holding Circuit
* Safety Interlock
* Toggle Control
* One-Shot / Rising Edge Detection
* Debounce Filtering
* TON Timer
* Pulse Timer Logic
* SR/RS Latch
* Counter CTU
* Lockout Timer
* Alarm Logic
* Horn ACK
* Overload Protection
* Multi-Motor Control
* HMI Parameter Input
* Industrial Ladder Structure

---

## 📺 Video hướng dẫn chi tiết

Video giải thích tư duy, nguyên lý hoạt động, mô phỏng và cách viết chương trình được cập nhật tại:

👉 https://www.youtube.com/@Electronics-GocnhinNguoidilam

Nội dung video tập trung vào:

* Giải thích logic từng Network
* Mô phỏng hoạt động chương trình
* Phân tích lỗi thường gặp
* Ứng dụng thực tế trong tủ điện và máy công nghiệp
* Cách viết Ladder Logic dễ hiểu, dễ bảo trì

---

## 📘 Tài liệu sắp hoàn thiện

Chúng tôi đang hoàn thiện tập 1 của tài liệu:

# PLC Thực Chiến – Thiết kế hệ thống điều khiển tự động dùng PLC theo chuẩn công nghiệp

Nếu các project trên GitHub giúp bạn **làm cho máy chạy**, thì tài liệu này sẽ giúp bạn:

* Thiết kế hệ thống theo chuẩn công nghiệp
* Đấu nối chống nhiễu đúng cách
* Bảo vệ thiết bị bền bỉ
* Xử lý lỗi thực tế ngoài hiện trường
* Tư duy từ thiết kế đến bàn giao
* Hiểu cách chọn thiết bị và tổ chức logic điều khiển

📌 Trạng thái: đang hoàn thiện.

Nếu bạn quan tâm, hãy ⭐ **Star repository** để không bỏ lỡ khi tài liệu được phát hành.

---

## 🧑‍🏭 Repository này phù hợp với ai?

Repository này phù hợp cho:

* Sinh viên ngành Điện – Tự động hóa
* Người mới học PLC
* Kỹ thuật viên bảo trì
* Kỹ sư điện công nghiệp
* Người học nghề tủ điện
* Người muốn chuyển sang mảng automation
* Người đã biết PLC cơ bản nhưng muốn viết logic thực tế hơn

---

## ✅ Cách sử dụng repository

Bạn có thể học theo thứ tự:

1. Đọc README tổng này
2. Vào từng thư mục bài học
3. Đọc README của từng bài
4. Mở file chương trình `.mwp`
5. Chạy mô phỏng hoặc nạp vào PLC
6. So sánh với video hướng dẫn
7. Tự mở rộng bài toán theo gợi ý

Khuyến nghị học theo thứ tự từ Bài 1 đến Bài 14 để nắm chắc nền tảng.

---

## 🔍 Keywords

PLC, Siemens S7-200, STEP 7 MicroWIN, Ladder Logic, PLC Programming, Industrial Automation, Motor Control, Start Stop, Toggle Control, TON Timer, Debounce, One-Shot, Counter, Alarm System, Overload, ACK, HMI, Factory Automation, Electrical Control, Automation Engineering

---

## 🤝 Nếu bạn thấy hữu ích

Bạn có thể ủng hộ project bằng cách:

* ⭐ Star repository
* 🍴 Fork repository
* 📺 Subscribe kênh YouTube
* 💬 Góp ý để cải thiện nội dung
* 📢 Chia sẻ cho cộng đồng học PLC

---

## 💬 Một lời chia sẻ thật lòng

PLC không quá khó.

Cái khó là:

* Giữ cho hệ thống ổn định
* Hiểu vì sao nó lỗi
* Biết cách sửa trong áp lực thực tế
* Biết cách thiết kế và lập trình theo đúng thực tế
* Đáp ứng đúng yêu cầu của chủ đầu tư
* Viết chương trình để người khác có thể đọc, sửa và bàn giao

Chúng tôi hy vọng những chia sẻ ở đây giúp bạn tiến nhanh hơn trên con đường học PLC và tự tin hơn khi đứng trước một tủ điện thật.

---

## 👷 Electro Industrial Works

**From the field – dành cho người đi làm.**

---

© James Dang – Electro Industrial Works
