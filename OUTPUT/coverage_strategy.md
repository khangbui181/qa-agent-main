# Coverage Strategy: Function D

| Vùng rủi ro | D-code | Khả năng | Căn cứ | Ảnh hưởng | Căn cứ | Ưu tiên |
|---|---|---|---|---|---|---|
| Tính tiền giảm và giới hạn khấu trừ | D02, D03, D11, D12, D20 | CAO | Nhiều điều kiện kết hợp (tỷ lệ phần trăm, trần giảm, làm tròn hàng nghìn 1.000đ, khấu trừ tổng gồm ship, chặn âm); còn mâu thuẫn M01, M04 | CAO | Trực tiếp gây sai tiền thanh toán, thất thoát doanh thu hoặc âm tiền đơn hàng | P1 |
| Quản lý lượt dùng và tranh chấp đồng thời | D05, D06, D14, D18 | CAO | Xử lý đồng thời (race condition) 2 khách đặt hàng cùng lúc ở lượt cuối; cơ chế tạm giữ lượt khi áp dụng và trừ khi đặt hàng; mâu thuẫn M02 | CAO | Bị vượt ngân sách khuyến mãi do lạm dụng mã quá tổng lượt sàn hoặc vượt 1 lượt/tài khoản | P1 |
| Trạng thái đơn hàng và thay đổi giỏ hàng | D15, D16, D17, D19 | CAO | Luồng chuyển đổi trạng thái phức tạp (sửa giỏ tụt ngưỡng, đổi số lượng, gỡ mã, hủy đơn hoàn lượt, mã bị admin tắt/hết hạn giữa chừng); mâu thuẫn M07 | CAO | Khách hưởng ưu đãi khi không còn đủ điều kiện, hoặc mất quyền lợi mã khi đơn bị hủy | P1 |
| Loại mã và phạm vi áp dụng | D01, D08 | CAO | Hỗ trợ 3 loại mã (% tiền hàng, cố định, Freeship từ 200.000đ); có mâu thuẫn mở M03 (ngưỡng 200k), M05 (áp toàn giỏ hay chặn khi có hàng ngoài phạm vi) | TB | Khách không áp được mã hoặc áp sai danh mục, phát sinh khiếu nại dịch vụ | P1 |
| Bảo mật và toàn vẹn dữ liệu | D21 | CAO | Không giới hạn số lần thử sai (nguy cơ brute-force); phản hồi gate dùng giá trị tiền giảm từ client gửi lên (lỗ hổng can thiệp giá M06) | CAO | Bị sửa request API can thiệp số tiền giảm, bị bot quét vét toàn bộ mã voucher | P1 |
| Quy tắc áp dụng và chuẩn hóa mã nhập | D07, D09, D10 | TB | Bắt buộc đăng nhập; tối đa 1 mã và tự động ghi đè mã mới; tự động auto-trim và chuyển chữ hoa | TB | Khách vãng lai thao tác không thành công hoặc nhầm lẫn khi mã cũ bị ghi đè | P2 |
| Thời gian hiệu lực và múi giờ | D04 | TB | Có giá trị biên thời gian (23:59:59 và 00:00:00 ngày hôm sau) và quy định múi giờ GMT+7 | TB | Khách mua hàng sát giờ bị từ chối hoặc mã hết hạn vẫn áp dụng được | P2 |
| Giao diện và trạng thái phản hồi | D13, D22 | TB | Thông báo lỗi riêng theo 4 nguyên nhân dưới ô nhập; hiển thị spinner và vô hiệu hóa nút Áp dụng | THAP | Sai sót hiển thị hoặc thiếu trạng thái chờ không gây thiệt hại tài chính | P3 |
| Lưu vết và thông tin đơn hàng | D23 | THAP | Hiển thị mã và số tiền giảm trong chi tiết đơn hàng, lịch sử và email xác nhận | THAP | Lỗi hiển thị sau mua không ảnh hưởng đến số tiền thực thu của đơn hàng | P3 |
| Phạm vi thị trường và tiền tệ | X01 | THAP | Quy định rõ chỉ hỗ trợ thị trường Việt Nam, tiền tệ VNĐ; loại trừ Nhật Bản | THAP | Phạm vi đã được xác định rõ ràng, không có rủi ro phát sinh ngoài luồng | P3 |

## Vùng P1

| Vùng | Loại test đề xuất | Kỹ thuật | Viewpoint | Mức độ |
|---|---|---|---|---|
| Tính tiền giảm và giới hạn khấu trừ | Functional Test, Boundary Test, Calculation Verification | Phân vùng tương đương (EP), Phân tích giá trị biên (BVA), Bảng quyết định (Decision Table) | VP-03 (Boundary), VP-08 (Integration) | Kiểm thử kỹ lưỡng toàn bộ các mốc biên (ngưỡng tối thiểu, trần giảm, phần lẻ 500đ/1.000đ, tiền giảm bằng tạm tính) |
| Quản lý lượt dùng và tranh chấp đồng thời | Concurrency Test, Security Test, Functional Test | Kiểm thử đồng thời (Concurrency Testing), Kiểm thử luồng nghiệp vụ (State Transition) | VP-04 (Security), VP-02 (Negative), VP-08 (Integration) | Giả lập nhiều tiến trình gửi yêu cầu cùng lúc ở lượt cuối; kiểm tra trừ lượt và chặn lượt tài khoản |
| Trạng thái đơn hàng và thay đổi giỏ hàng | Integration Test, End-to-End Test, Rollback Test | Kiểm thử chuyển đổi trạng thái (State Transition Testing), Use Case Testing | VP-08 (Integration), VP-02 (Negative) | Kiểm thử liên hoàn: Áp mã -> Sửa giỏ / Hết hạn phiên / Admin tắt mã -> Đặt hàng / Hủy đơn / Thanh toán thất bại |
| Loại mã và phạm vi áp dụng | Functional Test, Negative Test | Bảng quyết định (Decision Table), Phân vùng tương đương (EP) | VP-01 (Happy Path), VP-02 (Negative) | Kiểm thử cho từng loại mã (%, cố định, Freeship); kiểm thử giỏ hàng hỗn hợp theo phạm vi danh mục |
| Bảo mật và toàn vẹn dữ liệu | Security Test, Vulnerability Test | Thao tác sửa đổi tham số (Parameter Tampering), Thử sai liên tục (Brute-force simulation) | VP-04 (Security) | Kiểm tra máy chủ có tính lại tiền giảm hay nhận từ client; kiểm tra hành vi khi gửi mã sai hàng loạt |

## Chưa test được vì còn mâu thuẫn

| Vùng | Mâu thuẫn | Nguồn |
|---|---|---|
| Tính tiền giảm và giới hạn khấu trừ | M01: Tiền giảm vượt tạm tính và phí ship. Q-D12-01 chốt tiền giảm tối đa bằng tạm tính, nhưng Q-D20-01 chốt giảm trừ vào tổng đơn gồm ship. Chưa rõ phần dư có trừ tiếp vào phí ship hay không. | `knowledge/features/voucher.md` §7 M01, `OUTPUT/requirement_autopsy_report.md` |
| Tính tiền giảm và giới hạn khấu trừ | M04: Cơ sở tính tiền giảm %. Q-D20-01 chốt giảm vào tổng đơn gồm ship, nhưng chưa thống nhất tỷ lệ % tính trên tạm tính hay trên tổng có phí ship. | `knowledge/features/voucher.md` §7 M04, `OUTPUT/requirement_autopsy_report.md` |
| Quản lý lượt dùng và tranh chấp đồng thời | M02: Thời điểm giữ lượt dùng. Q-D14-01 chọn tạm giữ lượt khi bấm Áp dụng, trong khi Q-D18-01 chốt đơn ghi nhận trước tại máy chủ khi Đặt hàng mới thành công. | `knowledge/features/voucher.md` §7 M02, `OUTPUT/requirement_autopsy_report.md` |
| Loại mã và phạm vi áp dụng | M03: Điều kiện mã Freeship 200.000đ. Q-D01-01 đưa thêm điều kiện hóa đơn từ 200.000đ ngoài phương án đóng, chưa rõ so trên tạm tính hay tổng có phí ship. | `knowledge/features/voucher.md` §7 M03, `OUTPUT/requirement_autopsy_report.md` |
| Loại mã và phạm vi áp dụng | M05: Phạm vi áp dụng của mã. Q-D08-01 chốt áp dụng toàn bộ giỏ hàng, nhưng Q-D08-02 lại có quy tắc xử lý khi mã giới hạn phạm vi danh mục. | `knowledge/features/voucher.md` §7 M05, `OUTPUT/requirement_autopsy_report.md` |
| Bảo mật và toàn vẹn dữ liệu | M06: Toàn vẹn tiền giảm và rủi ro bảo mật. Q-D21-02 chốt dùng giá trị giao diện gửi lên, tạo rủi ro nghiêm trọng cho phép client can thiệp sửa tiền giảm. Cần BA/Tech Lead thẩm định lại. | `knowledge/features/voucher.md` §7 M06, `OUTPUT/requirement_autopsy_report.md` |
| Trạng thái đơn hàng và thay đổi giỏ hàng | M07: Lượt tạm giữ khi mất phiên hoặc gỡ mã. Q-D16-03 quy định mất mã khi hết hạn phiên nhưng chưa quy định cơ chế hoàn nhả lượt đã tạm giữ ở Q-D14-01. | `knowledge/features/voucher.md` §7 M07, `OUTPUT/requirement_autopsy_report.md` |
