### Báo cáo môn Kiến tập công nghiệp, với đề tài chính là TRIỂN KHAI HỆ THỐNG MẠNG DOANH NGHIỆP
Dự án này tập trung vào việc xây dựng một hệ thống mạng hoàn chỉnh, có dây và không dây, cho một công ty sản xuất với một trụ sở chính ở miền Nam và một chi nhánh ở miền Bắc, đồng thời đảm bảo các yếu tố về bảo mật và tính sẵn sàng của hạ tầng.
#### Phạm vi Triển khai: 
Hệ thống mạng được triển khai trên 2 site:
- Trụ sở chính (Miền Nam): Gồm 5 tầng với các phòng ban như Lễ tân, Nhân sự, Marketing, Kế toán, IT, Kiểm toán, Truyền thông, Phát triển Bền vững, Giám đốc và Phó Giám đốc.
- Chi nhánh (Miền Bắc): Gồm khu văn phòng (Tiếp tân, IT, Kế toán) và khu nhà máy sản xuất (Vận hành Sản xuất).
- Thiết bị: Sử dụng thiết bị mạng của Cisco (Router, Multilayer Switch, Switch Access) và 2 Server Windows để cung cấp các dịch vụ.
- Dịch vụ Máy chủ: Các máy chủ (Server) được cấu hình các dịch vụ thiết yếu như DHCP, Mail, DNS, Web, FTP, và AAA (trong đó server miền Nam đặt ở mạng 10.10.10.0/28 và server miền Bắc đặt ở mạng 10.10.11.0/28).
- Quy hoạch Địa chỉ IP và VLAN: Sử dụng kỹ thuật VLSM để phân vùng địa chỉ IP tĩnh cho khu vực Switch, Router, Server và gán các VLAN riêng biệt cho từng phòng ban (sử dụng VTP để quản lý VLAN).
#### Cấu hình Hạ tầng Cơ bản và Vận hành
- VLAN/VTP: Cấu hình VLAN và triển khai giao thức VTP (VLAN Trunking Protocol) ở các chế độ Server, Transparent, Client trên các Switch.
- Interface/Ethernet Channel: Cấu hình IP cho các cổng và sử dụng Ethernet Channel (Port-channel) giữa các Switch Layer 3 (Core/Distribution) để tăng băng thông và dự phòng.
- Định tuyến OSPF: Triển khai giao thức định tuyến động OSPF (Open Shortest Path First) để đảm bảo kết nối và tìm đường đi ngắn nhất giữa các mạng.
- Inter-VLAN Routing: Cấu hình định tuyến giữa các VLAN trên Switch Distribution.
#### Cấu hình Dự phòng và Mở rộng:
- HSRP: Cấu hình giao thức dự phòng Router HSRP (Hot Standby Router Protocol) trên các Switch Distribution.
- STP: Cấu hình Spanning Tree Protocol để chống lặp vòng lặp mạng.
#### Cấu hình Bảo mật Mạng:
- ACL & Firewall: Cấu hình danh sách kiểm soát truy cập (ACL) và tường lửa để kiểm soát lưu lượng.
- NAT & VPN: Cấu hình NAT (Network Address Translation) để ra Internet và VPN-IPSec để thiết lập kênh truyền thông bảo mật giữa hai site.
- Bảo mật Layer 2: Triển khai Port Security và DHCP Snooping trên các Switch Access để ngăn chặn tấn công.
- Quản trị Thiết bị: Cấu hình SSH Access để quản trị thiết bị từ xa một cách bảo mật, và NTP Syslog để đồng bộ thời gian và quản lý log hệ thống.

Dự án sử dụng 2 Server Windows (một ở miền Nam và một ở miền Bắc) và cấu hình các vai trò (Roles) máy chủ sau:
### 🖥️ Hạ tầng Máy chủ & Dịch vụ (Server Infrastructure)

Các dịch vụ mạng cốt lõi được triển khai tập trung tại Server Farm của hai miền, đảm bảo khả năng phục vụ liên tục cho toàn bộ hệ thống.

| Vai trò Máy chủ (Server Role) | Mục đích & Chức năng (Description) | Địa chỉ IP Triển khai (Deployment) |
| :--- | :--- | :--- |
| **DNS Server** | Phân giải tên miền thành địa chỉ IP (và ngược lại). Hỗ trợ duyệt web, gửi email nội bộ và truy cập dịch vụ. | **Miền Nam:** `10.10.10.2/28`<br>**Miền Bắc:** `10.10.11.2/28` |
| **DHCP Server** | Tự động cấp phát IP động, Subnet Mask, Gateway và DNS cho các máy Client trong các VLAN. | **Miền Nam:** `10.10.10.2/28`<br>**Miền Bắc:** `10.10.11.2/28` |
| **WEB Server** | Lưu trữ và phục vụ nội dung trang web nội bộ (Intranet) của công ty. | **Miền Nam:** `10.10.10.2/28`<br>**Miền Bắc:** `10.10.11.2/28` |
| **MAIL Server** | Quản lý dịch vụ thư điện tử (Email) để trao đổi thông tin liên lạc nội bộ giữa nhân viên. | **Miền Nam:** `10.10.10.2/28`<br>**Miền Bắc:** `10.10.11.2/28` |
| **FTP Server** | Cung cấp dịch vụ truyền tải tệp (File Transfer Protocol) phục vụ lưu trữ và chia sẻ dữ liệu. | **Miền Nam:** `10.10.10.2/28`<br>**Miền Bắc:** `10.10.11.2/28` |
| **RADIUS Server** | Cung cấp dịch vụ **AAA** (Authentication, Authorization, Accounting) để xác thực truy cập mạng. | **Miền Nam:** `10.10.10.2/28`<br>**Miền Bắc:** `10.10.11.2/28` |
