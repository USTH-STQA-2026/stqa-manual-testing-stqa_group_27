# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin | |
|---|---|
| **Nhóm** | `<!-- Tên nhóm -->` |
| **Ngày tạo** | `<!-- DD/MM/YYYY -->` |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

### IDM — Đăng nhập (REQ-01)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Email có tồn tại trong DB? | Có | `librarian@library.com` | Đăng nhập thành công |
| | Không | `noone@email.com` | Thông báo lỗi |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | Thông báo lỗi |
| Ô nhập có rỗng? | Không rỗng | (giá trị bất kỳ) | Xử lý bình thường |
| | Rỗng | `""` | Thông báo "Vui lòng nhập..." |

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Từ khóa có tồn tại trong DB? | Có (tên sách) | `"Flutter"` | Hiển thị sách chứa "Flutter" |
| | Có (tên tác giả) | `"Nguyễn"` | Hiển thị sách của tác giả Nguyễn |
| | Không | `"XYZ123"` | Danh sách rỗng |
| Phân biệt HOA/thường? | Chữ thường | `"flutter"` | Kết quả giống "Flutter" |
| | Chữ HOA | `"FLUTTER"` | Kết quả giống "Flutter" |

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Trạng thái sách? | Có sẵn | BOOK001 | Cho phép mượn |
| | Đang mượn | BOOK003 | Không cho phép |
| | Thất lạc | BOOK007 | Không cho phép |
| Trạng thái thành viên? | Hoạt động | MEM002 | Cho phép mượn |
| | Tạm ngưng | MEM004 | Từ chối, thông báo lỗi |
| | Hết hạn | MEM005 | Từ chối, thông báo lỗi |
| Số sách đang mượn? | < 3 (BVA: 0, 1, 2) | MEM006 (0 sách) | Cho phép mượn |
| | = 3 (BVA: giới hạn) | MEM đã mượn 3 sách | Từ chối, thông báo vượt giới hạn |

## IDM — Trả sách (REQ-05)

| Đặc tính (Characteristic)           | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi                            |
| ----------------------------------- | ----------------- | ------------------------ | ------------------------------------------- |
| Thành viên có đang mượn sách không? | Có                | MEM002 - BOOK003         | Cho phép trả sách                           |
| Thành viên có đang mượn sách không? | Không             | MEM002 - BOOK013         | Không cho phép trả                          |
| Tình trạng phiếu mượn               | Đúng hạn          | BR003                    | Trả thành công                              |
| Tình trạng phiếu mượn               | Quá hạn           | BR001                    | Trả thành công và hiển thị cảnh báo quá hạn |
| Trạng thái sách sau khi trả         | Đang mượn         | BOOK003                  | Chuyển thành "Có sẵn"                       |

## IDM — Xử lý sách quá hạn (REQ-06)

| Đặc tính (Characteristic) | Phân vùng (Block)           | Giá trị đại diện (Value) | Kết quả mong đợi                     |
| ------------------------- | --------------------------- | ------------------------ | ------------------------------------ |
| Ngày hết hạn (dueDate)    | <= ngày hiện tại            | BR001 (15/09/2024)       | Đánh dấu "Quá hạn"                   |
| Ngày hết hạn (dueDate)    | > ngày hiện tại             | BR003 (15/10/2024)       | Không đánh dấu quá hạn               |
| Vai trò người dùng        | Thủ thư                     | LIB001                   | Xem tất cả phiếu quá hạn             |
| Vai trò người dùng        | Thành viên                  | MEM002                   | Chỉ xem phiếu quá hạn của chính mình |
| Thao tác kiểm tra quá hạn | Nhấn nút "Kiểm tra quá hạn" | Librarian                | Hệ thống cập nhật trạng thái quá hạn |

## IDM — Quản lý thành viên (REQ-07)

| Đặc tính (Characteristic) | Phân vùng (Block)           | Giá trị đại diện (Value)                              | Kết quả mong đợi          |
| ------------------------- | --------------------------- | ----------------------------------------------------- | ------------------------- |
| Định dạng email           | Hợp lệ                      | [newuser@test.com](mailto:newuser@test.com)           | Tạo thành viên thành công |
| Định dạng email           | Thiếu ký tự @               | newusertest.com                                       | Hiển thị lỗi email        |
| Định dạng email           | Thiếu dấu chấm trong domain | newuser@test                                          | Hiển thị lỗi email        |
| Định dạng email           | Rỗng                        | ""                                                    | Hiển thị lỗi email        |
| Email đã tồn tại?         | Có                          | [librarian@library.com](mailto:librarian@library.com) | Báo lỗi trùng email       |
| Email đã tồn tại?         | Không                       | [abc123@test.com](mailto:abc123@test.com)             | Tạo thành viên thành công |
| Số điện thoại             | Hợp lệ                      | 0987654321                                            | Tạo thành viên thành công |
| Họ tên                    | Có dữ liệu                  | Nguyễn Văn A                                          | Tạo thành viên thành công |

## IDM — Tra cứu phiếu mượn (REQ-08)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi                     |
| ------------------------- | ----------------- | ------------------------ | ------------------------------------ |
| Vai trò người dùng        | Thủ thư           | LIB001                   | Xem tất cả phiếu mượn                |
| Vai trò người dùng        | Thành viên        | MEM002                   | Chỉ xem phiếu mượn của mình          |
| Member ID tra cứu         | Tồn tại           | MEM002                   | Hiển thị danh sách phiếu             |
| Member ID tra cứu         | Không tồn tại     | MEM999                   | Không có dữ liệu hoặc danh sách rỗng |
| Trạng thái phiếu          | Đang mượn         | BR003                    | Hiển thị đúng trạng thái             |
| Trạng thái phiếu          | Đã trả            | BR002                    | Hiển thị đúng trạng thái             |
| Trạng thái phiếu          | Quá hạn           | BR001                    | Hiển thị đúng trạng thái             |


## Decision Table — Borrow Book (REQ-04)

| Điều kiện | Rule 1 | Rule 2 | Rule 3 | Rule 4 |
|---|---|---|---|---|
| Book Available | Y | N | Y | Y |
| Member Active | Y | Y | N | Y |
| Borrow Count < 3 | Y | Y | Y | N |
| Result | Borrow | Reject | Reject | Reject |

> 💡 **Gợi ý kỹ thuật**: Sử dụng **Phân lớp tương đương (EP)** cho các phân vùng rời rạc, **Phân tích giá trị biên (BVA)** cho các phân vùng số (ví dụ: giới hạn 3 sách). Xem textbook §6.1–6.3.

---

## Bước 2: Test Cases

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| | | | | | | | |

---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
