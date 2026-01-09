# Bài 1 – Điều khiển chạy/dừng động cơ với liên động an toàn

## Mục tiêu
- Điều khiển động cơ chạy và dừng
- Nút Stop có ưu tiên
- Có liên động an toàn

## Ngõ vào
- I0.0 – Nút Start
- I0.1 – Nút Stop
- I0.2 – Nút E-STOP (Mở rộng)
- I0.3 - Cảm biến quá tải động cơ (Mở rộng)

## Ngõ ra
- Q0.0 – Contactor động cơ

## Yêu cầu
- Nhấn Start → động cơ chạy
- Nhấn Stop → động cơ dừng ngay
- Khi nhấn E-STOP → động cơ phải dừng ngay, không được chạy
- Khi quá tải → động cơ phải dừng ngay
