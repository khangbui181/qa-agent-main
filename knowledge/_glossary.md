# Thuật Ngữ Nghiệp Vụ Toàn Hệ Thống (_glossary.md)

> File này định nghĩa chính xác các thuật ngữ nghiệp vụ xuyên suốt mọi tính năng của dự án.
> Giúp Agent, Tester, BA và Developer hiểu cùng một nghĩa, tránh nhầm lẫn logic khi phân tích.

Dự án: ShopGo (Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §1) · Cập nhật: 2026-11-23

> Bảng bắt đầu trống. Chỉ ghi thuật ngữ có nguồn; agent không tự định nghĩa thay dự án.

---

## 1. Người Dùng & Phân Quyền (Users & Roles)

| Thuật ngữ | Tiếng Anh tương đương | Định nghĩa chính xác | Ranh giới / Lưu ý |
|---|---|---|---|
| Khách vãng lai | Guest | Xem sản phẩm, thêm vào giỏ; phải đăng nhập khi thanh toán | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §3 |
| Khách hàng | Customer | Đã đăng ký; có hồ sơ, sổ địa chỉ, Ví ShopGo, lịch sử đơn | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §3 |
| Nhân viên CSKH / Admin | Customer Support / Admin | Quản lý đơn, mã giảm giá, sản phẩm (back-office) | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §3 |

---

## 2. Thực Thể Nghiệp Vụ Cốt Lõi (Core Domain Entities)

| Thuật ngữ | Tiếng Anh tương đương | Định nghĩa chính xác | Ranh giới / Lưu ý |
|---|---|---|---|
| SKU | Stock Keeping Unit | Đơn vị phân loại hàng hóa / sản phẩm tồn kho | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |
| Tồn kho | Stock | Số lượng sản phẩm sẵn có trong kho để bán | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |
| COD | Cash on Delivery | Hình thức thanh toán thu tiền mặt khi nhận hàng | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |
| Voucher | Voucher / Discount code | Mã giảm giá theo tỷ lệ phần trăm (%) hoặc số tiền cố định (VNĐ) | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6, INPUT/Spec Function D.md §1 |
| Ví ShopGo | ShopGo E-Wallet | Ví điện tử lưu trữ số dư trả trước dùng để thanh toán trên ShopGo | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |
| Hoàn tiền | Refund | Hoàn tiền về ví ShopGo khi đơn bị hủy hoặc lỗi thanh toán | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6, gate_status.json Q-D15-01 |

---

## 3. Trạng Thái Vòng Đời (Lifecycle States)

| Trạng thái | Ý nghĩa | Hành vi hệ thống |
|---|---|---|
| Chờ xác nhận | Đơn hàng mới tạo, đang chờ xử lý | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |
| Đang giao | Đơn hàng đã giao cho đơn vị vận chuyển | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |
| Hoàn tất | Đơn hàng đã giao thành công và hoàn tất thanh toán | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |
| Đã hủy | Đơn hàng bị hủy | Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6 |

---

## 4. Tham Số & Công Thức Tính Toán (Parameters & Calculations)

| Thuật ngữ | Tiếng Anh tương đương | Định nghĩa & Công thức | Đơn vị / Định dạng |
|---|---|---|---|
| Giá trị đơn tối thiểu | Min order value | Giá trị đơn hàng quy định để mã giảm giá có hiệu lực (so sánh với tổng đơn hàng >= ngưỡng) | VNĐ (Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6, gate_status.json Q-D03-01, Q-D03-02) |
| Trần giảm tối đa | Max discount cap | Mức giảm tối đa áp dụng cho mã giảm giá theo tỷ lệ phần trăm (%) | VNĐ (Nguồn: INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §6, gate_status.json Q-D02-01) |