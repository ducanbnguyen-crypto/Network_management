🏫 Mô phỏng hệ thống mạng Trường Đại học Điện Lực
📘 Giới thiệu

Dự án này mô phỏng hệ thống mạng của Trường Đại học Điện Lực, bao gồm ba tòa chính:

Tòa A (ToaA) – Tuyến mạng chính ra Internet.

Tòa K (ToaK) – Tuyến dự phòng (backup) ra Internet.

Tòa E (ToaE) – Mạng nội bộ của toàn hệ thống.

Hệ thống được thiết kế nhằm đảm bảo tính ổn định, khả năng mở rộng và khả năng phục hồi cao, sử dụng các công nghệ tiên tiến như OSPF, định tuyến tĩnh, EtherChannel, VLAN, và DHCP.

🏗️ Cấu trúc hệ thống
🏢 Tòa A – Tuyến chính ra Internet

Định tuyến tĩnh 0.0.0.0/0 ra Internet qua interface GigabitEthernet1/0/1 (IP: 8.8.8.1).

Chạy giao thức OSPF với Router-ID: 3.3.3.3.

Kết nối đến Tòa E qua Port-channel1 (172.16.10.0/24).

Kết nối đến Tòa K qua Port-channel3 (172.16.30.0/24).

Phát tán default route vào OSPF (default-information originate).

🏢 Tòa K – Tuyến dự phòng (Backup)

Chạy OSPF với Router-ID: 2.2.2.2.

Kết nối đến Tòa E qua Port-channel2 (172.16.20.0/24).

Kết nối đến Tòa A qua Port-channel3 (172.16.30.0/24).

Tự động tiếp quản kết nối ra Internet khi Tòa A gặp sự cố.

🏢 Tòa E – Mạng nội bộ

Chạy OSPF với Router-ID: 1.1.1.1.

Cấu hình VLAN 10 → VLAN 100.

Mỗi VLAN có IP gateway nội bộ (interface vlanX) và cấu hình DHCP cấp phát động.

Kết nối:

Đến Tòa A qua Port-channel1 (172.16.10.0/24).

Đến Tòa K qua Port-channel2 (172.16.20.0/24).

Đảm nhiệm vai trò DHCP Server cấp phát IP cho các mạng con.

⚙️ Kỹ thuật áp dụng
Tính năng	Mô tả
OSPF	Giao thức định tuyến động, giúp các tòa tự động học bảng định tuyến.
Static Route	Cấu hình định tuyến tĩnh tại Tòa A ra Internet.
EtherChannel	Gộp nhiều liên kết vật lý thành một liên kết logic, tăng băng thông và độ dự phòng.
VLAN	Phân chia hệ thống mạng nội bộ thành các phân vùng logic riêng biệt.
DHCP Server	Tòa E cấp phát IP động cho từng VLAN.
NTP & Logging	Đồng bộ thời gian và ghi log hệ thống về địa chỉ 8.8.8.8.
📂 Cấu trúc thư mục
/mo_phong
│
├── readme.md        # Tài liệu mô tả hệ thống (file này)
├── ToaA.cfg         # Cấu hình Router Tòa A
├── ToaK.cfg         # Cấu hình Router Tòa K
├── ToaE.cfg         # Cấu hình Router Tòa E
└── so_do.pkt        # Sơ đồ mạng mô phỏng (Cisco Packet Tracer)

🧩 Kịch bản hoạt động

Mặc định, Tòa E truy cập Internet qua Tòa A (tuyến chính).

Khi Tòa A gặp sự cố, OSPF tự động tính toán lại tuyến và chuyển lưu lượng ra Internet qua Tòa K (tuyến dự phòng).

Các thiết bị đầu cuối được cấp IP từ DHCP Server (Tòa E) và sử dụng default gateway tương ứng với VLAN.

💻 Yêu cầu hệ thống

Cisco Packet Tracer 8.2+

Thiết bị mô phỏng tương thích với IOS 16.3.2

🧠 Ghi chú

Hệ thống có thể mở rộng thêm nhiều VLAN hoặc tuyến backup khác.

Cấu hình đã được kiểm thử thành công trong môi trường mô phỏng.

Có thể triển khai thực tế với các điều chỉnh phù hợp.

👨‍💻 Tác giả

Nguyễn Đức Anh – Sinh viên chuyên ngành Quản trị mạng, Trường Đại học Điện Lực
Hướng dẫn: Thầy Nguyễn Khánh Tùng
