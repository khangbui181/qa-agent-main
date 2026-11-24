# Project Knowledge — quy ước dùng cho MỌI feature

> Tri thức nền cấp **dự án**, do con người khai. Mọi skill đọc file này.
> Ô còn `<điền>` = **chưa khai báo**: agent coi như không có (`[CONTEXT_MISSING]` / `[CHƯA CÓ TRONG TÀI LIỆU]`),
> **không** tự lấy giá trị "thường gặp" thay vào. Xem `qa-system/core/QA_STANDARD.md` §8, §10.

Dự án: ShopGo (Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §1) · Cập nhật lần cuối: 2026-11-23

---

## 1. Quy ước định danh
| Đối tượng | Format | Ví dụ |
|---|---|---|
| Module prefix cho `TC_ID` (2–6 ký tự hoa) | VOUCHER (Nguồn: Kịch bản QA System) | VOUCHER-001 |
| Mã nghiệp vụ (mã đơn, mã giao dịch…) | `<điền>` | `<điền>` |

## 2. Định dạng dữ liệu
| Loại | Quy ước | Ghi chú |
|---|---|---|
| Ngày / Giờ | `<điền>` | Chưa khai thì agent dùng `YYYY-MM-DD` cho **tài liệu QA**, không suy ra định dạng hiển thị của hệ thống |
| Tiền tệ | VNĐ (Nguồn: INPUT/Spec Function D.md §1, INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §2, §7; gate_status.json Q-X01-01) | |
| Timezone | Asia/Ho_Chi_Minh (GMT+7) (Nguồn: gate_status.json Q-D04-02) | |
| NULL vs rỗng | `<điền>` | |

## 3. Bối cảnh nghiệp vụ dùng chung
> Dùng cho Risk Analysis (skill `01`). Ô chưa khai ⇒ `[CONTEXT_MISSING]`.

| Khía cạnh | Nội dung |
|---|---|
| User Context | `<điền>` |
| Usage Context | `<điền>` |
| Financial / Business Context | `<điền>` |
| Operational Context | `<điền>` |
| Criticality Context | `<điền>` |

## 4. Môi trường test
| Thuộc tính | Nội dung |
|---|---|
| Môi trường | `<điền>` |
| Cách seed data | `<điền>` |
| Có được dùng dữ liệu giống production? | `<điền>` |

## 5. Test Management Tool & ALM Integration
### 5.1. Jira Xray (Khuyến nghị)
| Thuộc tính | Cấu hình chuẩn | Ý nghĩa / Ghi chú |
|---|---|---|
| Issue Type | `Test` | Loại issue đại diện cho Test Case trong Xray |
| Summary Format | `[<TC_ID>] <Title>` | Ví dụ: `[AUTH-001] Verify đăng nhập thành công...` |
| Manual Steps | `Action`, `Data`, `Expected Result` | 3 cột chuẩn của bảng Manual Test Step trong Xray |
| Preconditions | `Preconditions` (Text/Wiki) | Tiền điều kiện trước khi thực hiện test |
| Priority | `High`, `Medium`, `Low` | Theo thang Priority của `QA_STANDARD.md` §11 |
| Labels | `Rule#BR-xx`, `Viewpoint#VP-xx`, `Module#<MOD>` | Phục vụ truy xuất nguồn gốc (Traceability) |
| File xuất | `OUTPUT/<slug>/export_jira_xray.csv` | Mã hóa UTF-8 BOM, sẵn sàng cho Xray Test Importer |

### 5.2. Redmine
| Thuộc tính | Cấu hình chuẩn | Ý nghĩa / Ghi chú |
|---|---|---|
| Tracker | `Test Case` (hoặc `Feature` / `Task`) | Phân loại issue trong Redmine |
| Subject | `[<TC_ID>] <Title>` | Tiêu đề test case |
| Priority | `Urgent` (High), `Normal` (Medium), `Low` (Low) | Mức độ ưu tiên chuẩn của Redmine |
| Category | Mã module (`AUTH`, `PAY`...) | Gom nhóm test case theo phân hệ |
| Status | `New` | Trạng thái ban đầu |
| Description | Đầy đủ Preconditions, Steps, Data, Expected Result | Định dạng Textile / Markdown của Redmine |
| File xuất | `OUTPUT/<slug>/export_redmine.csv` | Mã hóa UTF-8 BOM |

## 6. Ràng buộc riêng của dự án
> Quy tắc áp dụng xuyên suốt hệ thống, **có nguồn**. Chưa có thì để trống bảng.

| Quy tắc | Nguồn |
|---|---|
| Khách mua hàng và giao hàng tại Việt Nam, tiền tệ VNĐ, ngôn ngữ giao diện tiếng Việt | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §2, INPUT/Spec Function D.md §1, gate_status.json Q-X01-01, Q-X01-02 |
| Hiệu năng: trang chính tải < 3s | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §7 |
| Bảo mật: mật khẩu mã hóa, OTP có hạn | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §7 |
| Tương thích đa trình duyệt (Chrome, Edge, Safari bản mới) và responsive cho desktop + mobile web | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §5, §7 |
| Tiền tệ VND, làm tròn theo quy ước hiển thị (làm tròn số học đến hàng nghìn 1.000đ với phần lẻ mã %) | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §7, gate_status.json Q-D11-01 |
