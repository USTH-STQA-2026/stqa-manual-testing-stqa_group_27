# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin | |
|---|---|
| **Nhóm** | `Nhóm 27` |
| **Ngày tạo** | `22/04/2026 -->` |
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
| | Không | `no0ne@email.com` | Thông báo lỗi |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | Thông báo lỗi |
| Ô nhập có rỗng? | Không rỗng | (giá trị bất kỳ) | Xử lý bình thường |
| | Rỗng | `""` | Thông báo "Vui lòng nhập..." |

## IDM — Xem danh sách sách (REQ-02)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|------------|------------|------------|------------|
| Vai trò người dùng | Thủ thư | LIB001 | Xem được danh sách sách |
| Vai trò người dùng | Thành viên | MEM002 | Xem được danh sách sách |
| Trạng thái sách | Có sẵn | BOOK001 | Hiển thị "Có sẵn" |
| Trạng thái sách | Đã mượn | BOOK003 | Hiển thị "Đã mượn" |
| Trạng thái sách | Thất lạc | BOOK007 | Hiển thị "Thất lạc" |
| Thay đổi trạng thái | Sau khi mượn sách | BOOK001 | Cập nhật realtime thành "Đã mượn" |
| Thay đổi trạng thái | Sau khi trả sách | BOOK003 | Cập nhật realtime thành "Có sẵn" |

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

| Mã TC | Mục tiêu kiểm thử                              | Tiền điều kiện              | Bước thực hiện                                   | Dữ liệu đầu vào                                                   | Kết quả mong đợi                                      | REQ    | Kỹ thuật       |
| ----- | ---------------------------------------------- | --------------------------- | ------------------------------------------------ | ----------------------------------------------------------------- | ----------------------------------------------------- | ------ | -------------- |
| TC-01 | Đăng nhập thành công với tài khoản Thủ thư     | Đang ở màn hình Login       | 1. Nhập email 2. Nhập mật khẩu 3. Nhấn Đăng nhập | [librarian@library.com](mailto:librarian@library.com) / admin123  | Đăng nhập thành công, hiển thị tên và vai trò Thủ thư | REQ-01 | EP             |
| TC-02 | Đăng nhập với email không tồn tại              | Đang ở màn hình Login       | Nhập email không tồn tại và mật khẩu bất kỳ      | [noone@email.com](mailto:noone@email.com) / admin123              | Hiển thị "Không tìm thấy thành viên"                  | REQ-01 | EP             |
| TC-03 | Đăng nhập với mật khẩu sai                     | Đang ở màn hình Login       | Nhập email hợp lệ và mật khẩu sai                | [librarian@library.com](mailto:librarian@library.com) / wrongpass | Hiển thị "Mật khẩu không đúng"                        | REQ-01 | EP             |
| TC-04 | Đăng nhập khi bỏ trống thông tin               | Đang ở màn hình Login       | Để trống email và mật khẩu, nhấn Đăng nhập       | ""                                                                | Hiển thị "Vui lòng nhập email và mật khẩu"            | REQ-01 | EP             |
| TC-05 | Thành viên xem danh sách sách                  | Đăng nhập bằng Member       | Mở tab Sách                                      | MEM002                                                            | Hiển thị danh sách sách                               | REQ-02 | EP             |
| TC-06 | Thủ thư xem danh sách sách                     | Đăng nhập bằng Librarian    | Mở tab Sách                                      | LIB001                                                            | Hiển thị danh sách sách                               | REQ-02 | EP             |
| TC-07 | Trạng thái sách cập nhật sau khi mượn          | Có sách Available           | Mượn BOOK001                                     | BOOK001                                                           | Trạng thái đổi từ Có sẵn sang Đã mượn                 | REQ-02 | EP             |
| TC-08 | Tìm kiếm theo tên sách                         | Đăng nhập thành công        | Nhập từ khóa tìm kiếm                            | Flutter                                                           | Hiển thị sách có chứa Flutter                         | REQ-03 | EP             |
| TC-09 | Tìm kiếm theo tác giả                          | Đăng nhập thành công        | Tìm kiếm tác giả                                 | Nguyễn Minh Đức                                                   | Hiển thị sách của tác giả                             | REQ-03 | EP             |
| TC-10 | Tìm kiếm không phân biệt hoa thường            | Đăng nhập thành công        | Tìm kiếm                                         | flutter                                                           | Kết quả giống Flutter                                 | REQ-03 | EP             |
| TC-11 | Lọc theo thể loại                              | Đăng nhập thành công        | Chọn thể loại Công nghệ                          | Công nghệ                                                         | Chỉ hiển thị sách Công nghệ                           | REQ-03 | EP             |
| TC-12 | Tìm kiếm không có kết quả                      | Đăng nhập thành công        | Tìm kiếm từ khóa không tồn tại                   | XYZ123                                                            | Hiển thị "Không tìm thấy sách"                        | REQ-03 | EP             |
| TC-13 | Mượn sách thành công                           | Thành viên hoạt động        | Mượn BOOK001                                     | MEM002 + BOOK001                                                  | Tạo phiếu mượn thành công                             | REQ-04 | Decision Table |
| TC-14 | Không cho mượn sách đã được mượn               | BOOK003 đang được mượn      | Thực hiện mượn                                   | BOOK003                                                           | Từ chối mượn sách                                     | REQ-04 | Decision Table |
| TC-15 | Không cho mượn khi thành viên bị tạm ngưng     | Đăng nhập MEM004            | Mượn BOOK001                                     | MEM004                                                            | Thông báo từ chối do tạm ngưng                        | REQ-04 | Decision Table |
| TC-16 | Không cho mượn khi thành viên hết hạn          | Đăng nhập MEM005            | Mượn BOOK001                                     | MEM005                                                            | Thông báo từ chối do hết hạn                          | REQ-04 | Decision Table |
| TC-17 | Thành viên đang mượn 2 sách được mượn thêm     | Thành viên có 2 sách        | Mượn thêm 1 sách                                 | Borrow count = 2                                                  | Cho phép mượn                                         | REQ-04 | BVA            |
| TC-18 | Thành viên đã mượn 3 sách không được mượn thêm | Thành viên có 3 sách        | Mượn thêm sách                                   | Borrow count = 3                                                  | Từ chối, vượt giới hạn                                | REQ-04 | BVA            |
| TC-19 | Kiểm tra thông báo đúng lý do từ chối          | Thành viên MEM004 và MEM005 | Thử mượn sách                                    | MEM004/MEM005                                                     | Thông báo khác nhau cho tạm ngưng và hết hạn          | REQ-04 | Decision Table |
| TC-20 | Trả sách đang mượn thành công                  | thành viên đang mượn sách   | Trả BOOK003                                      | BOOK003                                                           | Trả thành công                                        | REQ-05 | EP             |
| TC-21 | Sau khi trả trạng thái sách cập nhật           | Đã trả sách                 | Kiểm tra danh sách sách                          | BOOK003                                                           | Trạng thái Có sẵn                                     | REQ-05 | EP             |
| TC-22 | Trả sách quá hạn                               | Có phiếu quá hạn            | Trả sách                                         | BR001                                                             | Hiển thị cảnh báo quá hạn                             | REQ-05 | EP             |
| TC-23 | Thủ thư thực hiện kiểm tra quá hạn             | Đăng nhập Librarian         | Nhấn "Kiểm tra quá hạn"                          | BR001                                                             | Hệ thống quét và cập nhật                             | REQ-06 | EP             |
| TC-24 | Phiếu quá hạn được đánh dấu đúng               | Đã chạy kiểm tra quá hạn    | Kiểm tra BR001                                   | BR001                                                             | Trạng thái = Quá hạn                                  | REQ-06 | EP             |
| TC-25 | Thành viên thấy phiếu quá hạn của mình         | Đăng nhập MEM002            | Mở danh sách phiếu mượn                          | BR001                                                             | Hiển thị trạng thái Quá hạn                           | REQ-06 | EP             |
| TC-26 | Thêm thành viên với email hợp lệ               | Đăng nhập Librarian         | Mở Thêm thành viên                               | [abc@test.com](mailto:abc@test.com)                               | Tạo thành công                                        | REQ-07 | EP             |
| TC-27 | Email thiếu @                                  | Đăng nhập Librarian         | Tạo thành viên                                   | abctest.com                                                       | Hiển thị lỗi email                                    | REQ-07 | EP             |
| TC-28 | Email thiếu dấu chấm trong domain              | Đăng nhập Librarian         | Tạo thành viên                                   | abc@test                                                          | Hiển thị lỗi email                                    | REQ-07 | EP             |
| TC-29 | Email hợp lệ chuẩn                             | Đăng nhập Librarian         | Tạo thành viên                                   | [abc@domain.com](mailto:abc@domain.com)                           | Tạo thành công                                        | REQ-07 | EP             |
| TC-30 | Không cho phép email trùng                     | Đăng nhập Librarian         | Tạo thành viên                                   | [librarian@library.com](mailto:librarian@library.com)             | Hiển thị lỗi trùng email                              | REQ-07 | EP             |
| TC-31 | Thủ thư xem tất cả phiếu mượn                  | Đăng nhập librarian         | Mở tab Mượn/Trả                                  | LIB001                                                            | Hiển thị tất cả phiếu                                 | REQ-08 | EP             |
| TC-32 | Thành viên chỉ xem phiếu của mình              | Đăng nhập MEM002            | Mở tab Mượn/Trả                                  | MEM002                                                            | Chỉ hiển thị phiếu của MEM002                         | REQ-08 | EP             |
| TC-33 | Thành viên không được xem phiếu người khác     | Đăng nhập MEM002            | Tra cứu MEM003                                   | MEM003                                                            | Không hiển thị dữ liệu của MEM003                     | REQ-08 | EP             |


---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |