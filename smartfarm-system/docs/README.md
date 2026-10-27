# SmartFarm System

Hệ thống quản lý và giám sát tự động cho nông trại thông minh (SmartFarm).

---

## Hướng dẫn chạy script

Mở PowerShell tại thư mục gốc của dự án và thực hiện theo các bước sau:

```powershell
# Di chuyển vào thư mục scripts
cd scripts

# Bỏ qua chính sách ExecutionPolicy và chạy robot lọc log tự động
.\auto_report.ps1
```

---

## Checklist kiểm tra thiết bị

- [x] Kiểm tra nguồn điện trạm trung tâm
- [ ] Kiểm tra kết nối mạng cảm biến `sensor-03`
- [ ] Vệ sinh và hiệu chuẩn cảm biến độ ẩm đất

---

## Bảng thông số phần cứng

| Tên thiết bị | Đơn giá (VNĐ) | Loại Storage |
| :--- | :--- | :--- |
| Cảm biến độ ẩm DHT22 | 150,000 | Flash Memory |
| Modem Gateway 4G | 1,200,000 | NVMe SSD |
| Bộ điều khiển trung tâm ESP32 | 250,000 | MicroSD |

---

## Phân tích hệ thống

### 1. Mô hình Input – Process – Output
* **Input (Đầu vào):** File nhật ký hệ thống `log-he-thong.log` chứa các thông điệp ghi nhận trạng thái hoạt động (INFO, WARNING, ERROR, CRITICAL).
* **Process (Xử lý):** Script `auto_report.ps1` đọc dữ liệu log bằng `Get-Content`, lọc các chuỗi ký tự chứa từ khóa `"CRITICAL"` hoặc `"ERROR"` thông qua `Select-String`, sau đó đếm tổng số dòng sự cố lọc được bằng `Measure-Object`.
* **Output (Đầu ra):** Báo cáo sự cố được tự động sinh ra và ghi vào file `backup/bao-cao-su-co.txt` bao gồm: Tiêu đề báo cáo, Ngày giờ chạy script, Danh sách chi tiết các dòng lỗi, và Dòng tổng kết số lượng sự cố.

### 2. Phân loại theo Mô hình DIKW

* **Data (Dữ liệu - Dạng dữ liệu thô, chưa qua xử lý):**
  > **(b)** `2026-10-12 08:15:01 [ERROR] sensor-03 Connection TIMEOUT after 30s`
* **Information (Thông tin - Dữ liệu đã được tổng hợp, xử lý có bối cảnh):**
  > **(c)** *"Hôm nay hệ thống ghi nhận 8 sự cố, trong đó 4 sự cố đến từ trạm sensor-03."*
* **Knowledge (Tri thức - Hiểu biết rút ra từ thông tin tích lũy):**
  > **(d)** *"Trạm sensor-03 hay mất kết nối vào buổi chiều do sóng yếu, nên cần lắp thêm anten."*
* **Wisdom (Trí tuệ - Quyết định, hành động mang tính chiến lược):**
  > **(a)** *"Quyết định thay modem trạm sensor-03 trước thứ Sáu."*