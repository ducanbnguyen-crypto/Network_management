# 🖧 Mô phỏng hệ thống mạng trường Đại học Điện Lực

## 🏢 Giới thiệu

Dự án này mô phỏng một hệ thống mạng bao gồm ba thành phần chính:

- **Tòa A (ToaA)**: Tuyến mạng chính ra Internet.
- **Tòa K (ToaK)**: Tuyến dự phòng (backup) ra Internet.
- **Tòa E (ToaE)**: Mạng nội bộ.

Hệ thống sử dụng các kỹ thuật định tuyến và chuyển mạch tiên tiến như OSPF, định tuyến tĩnh, EtherChannel và phân chia VLAN để mô phỏng một mạng có tính ổn định, khả năng mở rộng và phục hồi cao.

---

## 🧩 Cấu trúc hệ thống

### 1. **ToaA – Tuyến chính ra Internet**
- Định tuyến tĩnh `0.0.0.0/0` ra Internet qua interface `GigabitEthernet1/0/1` (IP 8.8.8.1).
- Chạy giao thức OSPF với Router-ID `3.3.3.3`.
- Giao tiếp với ToaE (qua Port-channel1: 172.16.10.0/24).
- Giao tiếp với ToaK (qua Port-channel3: 172.16.30.0/24).
- Phát tán default route (`default-information originate`).

### 2. **ToaK – Tuyến backup ra Internet**
- Chạy OSPF với Router-ID `2.2.2.2`.
- Kết nối với ToaE qua Port-channel2: 172.16.20.0/24.
- Kết nối với ToaA qua Port-channel3: 172.16.30.0/24.
- Sẵn sàng tiếp quản kết nối ra Internet khi ToaA gặp sự cố.

### 3. **ToaE – Mạng nội bộ**
- Chạy OSPF với Router-ID `1.1.1.1`.
- Cấu hình các VLAN từ VLAN10 đến VLAN100.
- Mỗi VLAN có một địa chỉ IP gateway nội bộ (`interface vlanX`) và cấp phát DHCP tương ứng.
- Kết nối đến ToaA qua Port-channel1 (172.16.10.0/24).
- Kết nối đến ToaK qua Port-channel2 (172.16.20.0/24).
- Cấu hình DHCP Server đầy đủ cho các mạng con.

---

## ⚙️ Kỹ thuật đã áp dụng

| Tính năng         | Mô tả                                                                 |
|-------------------|-----------------------------------------------------------------------|
| **OSPF**          | Giao thức định tuyến động để trao đổi bảng định tuyến giữa các tòa. |
| **Static Route**  | Cấu hình tại ToaA để ra Internet (`ip route 0.0.0.0 0.0.0.0`).       |
| **EtherChannel**  | Gộp nhiều liên kết vật lý thành một liên kết logic để tăng băng thông và dự phòng. |
| **VLAN**          | Phân chia hệ thống mạng nội bộ thành các phân vùng logic riêng biệt. |
| **DHCP Server**   | ToaE cấp phát địa chỉ IP động cho từng VLAN.                         |
| **NTP & Logging** | Đồng bộ thời gian và ghi log hệ thống về địa chỉ 8.8.8.8.             |

---

## 📁 Nội dung thư mục

```
/mo phong
│
├── readme.md                  # Tài liệu mô tả hệ thống (file này)
├── ToaA.cfg                   # Cấu hình của router Tòa A
├── ToaK.cfg                   # Cấu hình của router Tòa K
├── ToaE.cfg                   # Cấu hình của router Tòa E
└── so_do.pkt                  # File sơ đồ mạng mô phỏng bằng Cisco Packet Tracer
```

---

## 🔁 Kịch bản hoạt động

- Mặc định, ToaE sẽ truy cập Internet thông qua ToaA.
- Khi ToaA mất kết nối, OSPF sẽ tự động tính lại tuyến và chuyển lưu lượng ra Internet qua ToaK.
- Các thiết bị đầu cuối được cấp phát IP thông qua DHCP từ ToaE và sử dụng default gateway theo từng VLAN.

---

## 🚀 Yêu cầu hệ thống

- **Cisco Packet Tracer 8.2+**
- Thiết bị mô phỏng tương thích với IOS 16.3.2

---

## 📌 Ghi chú

- Hệ thống có thể mở rộng thêm nhiều VLAN hoặc tuyến backup khác.
- Cấu hình đã thử nghiệm thành công trong môi trường mô phỏng và có thể triển khai thực tế với các điều chỉnh phù hợp.

---

> 👨‍💻 Tác giả: Phạm Quang Thắng – Sinh viên chuyên ngành Quản trị mạng  
> ✉️ Hướng dẫn: Thầy Nguyễn Khánh Tùng – Trường Đại học Điện lực
