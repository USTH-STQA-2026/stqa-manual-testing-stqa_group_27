# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin | |
|---|---|
| **Nhóm** | `<!-- Tên nhóm -->` |
| **Ngày báo cáo** | `<!-- DD/MM/YYYY -->` |

---

## BUG-06

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-06 |
| **TC liên quan** | TC-18 |
| **REQ liên quan** | REQ-04 |
| **Mức độ** | Medium|
| **Người phát hiện** | NGUYỄN NGỌC TRUNG|
| **Ngày phát hiện** | 29/05/2026 |
| **Trạng thái** | Closed |

**Tiêu đề:**
Có 3 sách vẫn mượn thêm được 1 sách

**Môi trường:**
- Trình duyệt: Chrome 
- Hệ điều hành: OS
- Ngôn ngữ giao diện: Tiếng Việt

**Điều kiện tiên quyết:**
Thành viên có 3 sách

**Bước tái hiện:**
1. Mượn thêm sách

**Kết quả mong đợi:**
Từ chối, vượt giới hạn

**Kết quả thực tế:**
`vẫn mượn thêm được 1 sách

**Tác động:**
VD: Vi phạm quy tắc nghiệp vụ cốt lõi, cho phép mượn vượt giới hạn

**Minh chứng:**

**Đề xuất xử lý:**
Kiểm tra điều kiện số lượng sách đang mượn trước khi tạo phiếu mượn mới

---

## BUG-07

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-07 |
| **TC liên quan** | TC-19 |
| **REQ liên quan** | REQ-04 |
| **Mức độ** | Medium |
| **Người phát hiện** | Nguyễn Ngọc Trung |
| **Ngày phát hiện** | 29/05/2026 |
| **Trạng thái** | Closed |

**Tiêu đề:**
Kiểm tra thông báo đúng lý do từ chối

**Bước tái hiện:**
1. Thử mượn sách

**Kết quả mong đợi:**
Thông báo khác nhau cho tạm ngưng và hết hạn

**Kết quả thực tế:**
cả 2 MEM hiện thông báo giống nhau

**Tác động:**
Gây nhầm lẫn giữa trường hợp tài khoản tạm ngưng và tài khoản hết hạn

**Minh chứng:**
`<!-- -->`

**Đề xuất xử lý:**
Xây dựng thông báo riêng cho từng trạng thái thành viên

---

# BUG-08

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-08 |
| **TC liên quan** | TC-22 |
| **REQ liên quan** | `REQ-05 |
| **Mức độ** | Medium |
| **Người phát hiện** | Nguyễn Ngọc Trung |
| **Ngày phát hiện** | 29/05/2026 |
| **Trạng thái** | Closed |

*Tiêu đề:*
Trả sách quá hạn

*Bước tái hiện:*
1. Trả sách

*Kết quả mong đợi:*
Hiển thị cảnh báo quá hạn

*Kết quả thực tế:*
không hiển thị cảnh báo quá hạn

*Tác động:*
Có thể bỏ sót việc tính phí phạt hoặc ghi nhận vi phạm.

*Minh chứng:*
<!-- -->

*Đề xuất xử lý:*
Khi ngày trả thực tế lớn hơn ngày phải trả, hệ thống phải hiển thị cảnh báo quá hạn.

---

# BUG-09

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-09 |
| **TC liên quan** | TC-24 |
| **REQ liên quan** | REQ-06 |
| **Mức độ** | Medium |
| **Người phát hiện** | Nguyễn Ngọc Trung |
| **Ngày phát hiện** | 29/05/2026 |
| **Trạng thái** | Closed |

*Tiêu đề:*
Phiếu quá hạn được đánh dấu đúng

*Bước tái hiện:*
1. Kiểm tra BR001

*Kết quả mong đợi:*
Trạng thái = Quá hạn

*Kết quả thực tế:*
không hiển thị trạng thái quá hạn

*Tác động:*
Dễ bỏ sót các trường hợp cần nhắc nhở hoặc xử lý vi phạm

*Minh chứng:*
<!-- -->

*Đề xuất xử lý:*
Tự động cập nhật trạng thái phiếu mượn thành "Quá hạn" khi ngày hiện tại vượt quá hạn trả.

---

# BUG-10

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-10 |
| **TC liên quan** | TC-25 |
| **REQ liên quan** | REQ-06 |
| **Mức độ** | Medium |
| **Người phát hiện** | Nguyễn Ngọc Trung |
| **Ngày phát hiện** | 29/05/2026 |
| **Trạng thái** | Closed |

*Tiêu đề:*
Phiếu quá hạn được đánh dấu đúng

*Bước tái hiện:*
1. Đã chạy kiểm tra quá hạn

*Kết quả mong đợi:*
Trạng thái = Quá hạn

*Kết quả thực tế:*
không hiển thị trạng thái quá hạn

*Tác động:*
Các phiếu mượn quá hạn không được đánh dấu, dẫn đến sai lệch dữ liệu quản lý

*Minh chứng:*
<!-- -->

*Đề xuất xử lý:*
Sau khi chạy kiểm tra, hệ thống phải cập nhật trạng thái các phiếu đủ điều kiện sang "Quá hạn"

---
