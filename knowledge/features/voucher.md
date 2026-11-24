# Function D: Áp dụng Mã Giảm Giá (Voucher) — Feature Knowledge

> Dữ kiện nghiệp vụ **có nguồn** của một tính năng. Mọi agent đọc file này trước khi chạy.
> Luật đọc/ghi: `qa-system/core/QA_STANDARD.md` §8. Schema bảng: §9.
>
> File được tạo tự động khi intake (hoặc `npm run knowledge:new <feature-slug>`) và **bắt đầu trống**.
> Dòng còn `<điền>` hoặc bảng chưa có dòng = chưa khai báo — agent coi như không có, không tự điền giá trị.

Feature slug: `voucher` · Nguồn tài liệu: `INPUT/Spec Function D.md`, `INPUT/Hệ thống Thương mại điện tử “ShopGo”.md`, `gate_status.json` · Cập nhật lần cuối: `2026-11-23` · Trạng thái tổng thể: `CONFIRMED`

---

## 1. TỔNG QUAN TÍNH NĂNG (FEATURE OVERVIEW)
Function D: Áp dụng Mã Giảm Giá (Voucher) tại bước Thanh toán của hệ thống Thương mại điện tử ShopGo. (Nguồn: INPUT/Spec Function D.md §1)

## 2. TÁC NHÂN & PHÂN QUYỀN (ACTORS & ROLES)

| Tác nhân (Actor) | Quyền hạn trong tính năng này | Điều kiện tiên quyết | Nguồn (file §mục) |
|---|---|---|---|
| Khách hàng (Customer) | Nhập mã giảm giá và áp dụng vào đơn hàng tại trang Thanh toán | Bắt buộc đăng nhập tài khoản trước khi nhập mã | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §3, gate_status.json Q-D07-01 |
| Khách vãng lai (Guest) | Xem sản phẩm, thêm vào giỏ; chưa được áp dụng mã | Phải đăng nhập/đăng ký tài khoản để thanh toán và áp mã | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §3, gate_status.json Q-D07-01 |

## 3. QUY TẮC NGHIỆP VỤ ĐÃ XÁC NHẬN (CONFIRMED BUSINESS RULES)
> Chỉ rule **có nguồn**: tài liệu (kèm trích nguyên văn) hoặc quyết định ở Mục 8 (`knowledge §8 #n`).
> Điều chưa rõ nằm ở Mục 7.

| ID | Điều kiện | Hành vi / Kết quả | Nguồn (file §mục) | Trích nguyên văn | Trạng thái |
|---|---|---|---|---|---|
| BR-01 | Khách chọn loại mã | Có 2 loại mã: giảm theo phần trăm (%) và giảm số tiền cố định (VND). | INPUT/Spec Function D.md §1 | Có 2 loại mã: giảm theo phần trăm (%) và giảm số tiền cố định (VND). | CONFIRMED |
| BR-02 | Khách áp dụng mã | Mã chỉ dùng được khi đơn hàng đạt giá trị tối thiểu quy định của mã. | INPUT/Spec Function D.md §1 | Mã chỉ dùng được khi đơn hàng đạt giá trị tối thiểu quy định của mã. | CONFIRMED |
| BR-03 | Kiểm tra hạn dùng | Mỗi mã có ngày hết hạn. | INPUT/Spec Function D.md §1 | Mỗi mã có ngày hết hạn. | CONFIRMED |
| BR-04 | Áp dụng mã hợp lệ | Áp dụng thành công thì hiển thị số tiền được giảm và tổng tiền mới. | INPUT/Spec Function D.md §1 | Áp dụng thành công → hiển thị số tiền được giảm và tổng tiền mới. | CONFIRMED |
| BR-05 | Mã không hợp lệ hoặc hết hạn | Hiển thị thông báo lỗi. | INPUT/Spec Function D.md §1 | Mã không hợp lệ hoặc hết hạn → hiển thị thông báo lỗi. | CONFIRMED |
| R06 | Q-D01-01 (D01) | Ô "Mã giảm giá" có hỗ trợ mã Freeship (mặc định áp dụng cho hóa đơn từ 200.000đ). | gate_status.json (Q-D01-01) | A. Có hỗ trợ mã Freeship (nhưng mặc định áp dụng cho hóa đơn từ 200.000đ) | CONFIRMED |
| R07 | Q-D02-01 (D02) | Mã % có trần giảm tối đa, cấu hình theo từng mã. | gate_status.json (Q-D02-01) | A. Có trần giảm tối đa (cấu hình theo từng mã cụ thể) | CONFIRMED |
| R08 | Q-D03-01 (D03) | Giá trị đơn tối thiểu so trên tổng tiền đơn hàng đã bao gồm phí vận chuyển. | gate_status.json (Q-D03-01) | B. Tổng tiền đơn hàng (đã bao gồm phí vận chuyển) | CONFIRMED |
| R09 | Q-D03-02 (D03) | "Đạt giá trị tối thiểu" là lớn hơn hoặc bằng (>=) giá trị tối thiểu quy định. | gate_status.json (Q-D03-02) | A. Lớn hơn hoặc bằng (>= giá trị tối thiểu quy định) | CONFIRMED |
| R10 | Q-D04-01 (D04) | Mã có hiệu lực đến 23:59:59 của ngày hết hạn. | gate_status.json (Q-D04-01) | A. Đến 23:59:59 của ngày hết hạn | CONFIRMED |
| R11 | Q-D04-02 (D04) | Hạn mã kiểm tra theo múi giờ Việt Nam (GMT+7). | gate_status.json (Q-D04-02) | A. Múi giờ Việt Nam (GMT+7) | CONFIRMED |
| R12 | Q-D05-01 (D05) | Mã có giới hạn tổng lượt dùng toàn hệ thống; hết lượt thì không dùng được nữa. | gate_status.json (Q-D05-01) | A. Có giới hạn tổng lượt dùng toàn hệ thống (hết lượt thì không dùng được nữa) | CONFIRMED |
| R13 | Q-D06-01 (D06) | Mỗi tài khoản khách hàng áp dụng thành công một mã tối đa 1 lần. | gate_status.json (Q-D06-01) | A. Tối đa 1 lần duy nhất cho mỗi tài khoản khách hàng | CONFIRMED |
| R14 | Q-D07-01 (D07) | Bắt buộc đăng nhập tài khoản trước mới được nhập và áp dụng mã. | gate_status.json (Q-D07-01) | A. Bắt buộc phải đăng nhập tài khoản trước mới được nhập và áp dụng mã | CONFIRMED |
| R15 | Q-D08-01 (D08) | Mã áp dụng cho toàn bộ sản phẩm trong giỏ hàng. | gate_status.json (Q-D08-01) | A. Áp dụng cho toàn bộ sản phẩm trong giỏ hàng | CONFIRMED |
| R16 | Q-D08-02 (D08) | Mã giới hạn phạm vi, giỏ có cả sản phẩm thuộc và không thuộc phạm vi: không cho áp mã. | gate_status.json (Q-D08-02) | C. Không cho áp mã | CONFIRMED |
| R17 | Q-D09-01 (D09) | Tối đa 1 mã giảm giá trên một đơn hàng. | gate_status.json (Q-D09-01) | A. Tối đa 1 mã giảm giá duy nhất trên một đơn hàng | CONFIRMED |
| R18 | Q-D09-02 (D09) | Đã áp mã A, nhập mã B hợp lệ thì mã B tự động ghi đè thay mã A. | gate_status.json (Q-D09-02) | A. Tự động ghi đè mã mới thay thế mã cũ nếu mã mới hợp lệ | CONFIRMED |
| R19 | Q-D10-01 (D10) | Mã không phân biệt hoa/thường, hệ thống tự chuyển thành chữ in hoa. | gate_status.json (Q-D10-01) | A. Không phân biệt hoa/thường (hệ thống tự động chuyển đổi thành chữ in hoa) | CONFIRMED |
| R20 | Q-D10-02 (D10) | Hệ thống tự cắt khoảng trắng thừa đầu/cuối trước khi kiểm tra mã. | gate_status.json (Q-D10-02) | A. Tự động cắt bỏ khoảng trắng thừa (auto-trim) trước khi kiểm tra mã | CONFIRMED |
| R21 | Q-D11-01 (D11) | Tiền giảm của mã % có phần lẻ được làm tròn số học đến hàng nghìn 1.000đ. | gate_status.json (Q-D11-01) | C. Làm tròn số học đến hàng nghìn 1.000đ | CONFIRMED |
| R22 | Q-D12-01 (D12) | Mã cố định lớn hơn tạm tính: tiền giảm tối đa bằng tạm tính, tiền hàng sau giảm về 0đ. | gate_status.json (Q-D12-01) | A. Tiền giảm tối đa bằng tạm tính tiền hàng (tiền hàng sau giảm về 0đ) | CONFIRMED |
| R23 | Q-D13-01 (D13) | Thông báo lỗi tách theo nguyên nhân: Hết hạn, Chưa đủ đơn tối thiểu, Hết lượt dùng, Mã không tồn tại. | gate_status.json (Q-D13-01) | A. Tách biệt thông báo lỗi theo nguyên nhân (Hết hạn, Chưa đủ đơn tối thiểu, Hết lượt dùng, Mã không tồn tại) | CONFIRMED |
| R24 | Q-D13-02 (D13) | Thông báo lỗi hiển thị ngay dưới ô "Mã giảm giá". | gate_status.json (Q-D13-02) | A. Ngay dưới ô "Mã giảm giá" | CONFIRMED |
| R25 | Q-D14-01 (D14) | Tạm giữ lượt khi Áp dụng mã thành công, trừ chính thức khi Đặt hàng thành công. | gate_status.json (Q-D14-01) | B. Tạm giữ lượt khi bấm Áp dụng mã thành công và trừ chính thức khi Đặt hàng thành công | CONFIRMED |
| R26 | Q-D15-01 (D15) | Đơn có mã bị hủy hoặc thanh toán thẻ thất bại: tự động hoàn lại lượt dùng mã. | gate_status.json (Q-D15-01) | A. Tự động hoàn lại lượt dùng mã cho khách hàng | CONFIRMED |
| R27 | Q-D15-02 (D15) | Thanh toán thẻ/Ví ShopGo thất bại: hoàn lượt, khách áp lại được. | gate_status.json (Q-D15-02) | A. Hoàn lượt, khách áp lại được | CONFIRMED |
| R28 | Q-D16-01 (D16) | Sửa giỏ làm đơn dưới mức tối thiểu: tự động hủy áp dụng mã và thông báo đơn không còn đủ điều kiện hưởng ưu đãi. | gate_status.json (Q-D16-01) | A. Tự động hủy áp dụng mã và thông báo đơn hàng không còn đủ điều kiện hưởng ưu đãi | CONFIRMED |
| R29 | Q-D16-02 (D16) | Đã áp mã %, đổi số lượng (vẫn đạt ngưỡng): tiền giảm tự tính lại theo giỏ mới. | gate_status.json (Q-D16-02) | A. Tự tính lại theo giỏ mới | CONFIRMED |
| R30 | Q-D16-03 (D16) | Phiên đăng nhập hết hạn sau khi áp mã: mất mã, khách phải áp lại. | gate_status.json (Q-D16-03) | B. Mất mã, khách phải áp lại | CONFIRMED |
| R31 | Q-D17-01 (D17) | Có nút hoặc biểu tượng gỡ mã; bấm thì gỡ mã và tính lại tổng tiền ban đầu. | gate_status.json (Q-D17-01) | A. Có nút hoặc biểu tượng gỡ mã, khi bấm vào thì gỡ mã và tính lại tổng tiền ban đầu | CONFIRMED |
| R32 | Q-D18-01 (D18) | Mã còn 1 lượt, 2 khách cùng Đặt hàng: đơn ghi nhận trước tại máy chủ thành công, đơn sau báo lỗi mã đã hết lượt. | gate_status.json (Q-D18-01) | A. Đơn hàng nào ghi nhận trước tại máy chủ sẽ áp dụng thành công, đơn hàng sau báo lỗi mã đã hết lượt | CONFIRMED |
| R33 | Q-D19-01 (D19) | Kiểm tra lại mã khi bấm Đặt hàng; mã hết hạn hoặc bị Admin vô hiệu hóa thì từ chối tạo đơn và thông báo mã không còn hiệu lực. | gate_status.json (Q-D19-01) | A. Kiểm tra lại tại thời điểm bấm Đặt hàng, từ chối tạo đơn và thông báo mã không còn hiệu lực | CONFIRMED |
| R34 | Q-D20-01 (D20) | Tiền giảm (% và cố định) trừ vào tổng giá trị đơn hàng, gồm cả tiền hàng và phí vận chuyển. | gate_status.json (Q-D20-01) | B. Giảm trừ vào tổng giá trị đơn hàng (bao gồm cả tiền hàng và phí vận chuyển) | CONFIRMED |
| R35 | Q-D20-02 (D20) | Đổi địa chỉ giao làm thay đổi phí ship sau khi áp mã: mã được kiểm tra và tính lại. | gate_status.json (Q-D20-02) | A. Có, kiểm tra và tính lại | CONFIRMED |
| R36 | Q-D21-01 (D21) | Không giới hạn số lần nhập mã sai. | gate_status.json (Q-D21-01) | B. Không giới hạn số lần nhập mã sai | CONFIRMED |
| R37 | Q-D21-02 (D21) | Tiền giảm dùng giá trị giao diện gửi lên. (Còn mở, xem M06) | gate_status.json (Q-D21-02) | B. Dùng giá trị giao diện gửi lên | CONFIRMED |
| R38 | Q-D22-01 (D22) | Bấm Áp dụng: hiển thị spinner/loading và vô hiệu hóa nút Áp dụng trong khi chờ API phản hồi. | gate_status.json (Q-D22-01) | A. Có hiển thị spinner/loading và vô hiệu hóa nút Áp dụng trong khi chờ API phản hồi | CONFIRMED |
| R39 | Q-D23-01 (D23) | Lưu vết đầy đủ mã và số tiền được giảm, hiển thị trong chi tiết đơn, lịch sử đơn và email xác nhận. | gate_status.json (Q-D23-01) | A. Có lưu vết đầy đủ mã giảm giá, số tiền được giảm và hiển thị trong chi tiết đơn hàng, lịch sử đơn và email xác nhận | CONFIRMED |
| R40 | Q-X01-01 (X01) | Chỉ phục vụ khách mua hàng và giao hàng tại Việt Nam, tiền tệ VNĐ. | gate_status.json (Q-X01-01) | A. Không hỗ trợ, chỉ phục vụ khách mua hàng và giao hàng tại Việt Nam với đơn vị tiền tệ VNĐ | CONFIRMED |
| R41 | Q-X01-02 (X01) | Không áp dụng tiền tệ, múi giờ Nhật Bản vì chỉ bán tại Việt Nam. | gate_status.json (Q-X01-02) | C. Không áp dụng vì chỉ bán tại Việt Nam | CONFIRMED |

### 3b. FIELD & RÀNG BUỘC

| ID | Field | Kiểu | Bắt buộc | Ràng buộc (min/max/định dạng) | Thông báo khi sai | Nguồn (file §mục) | Trích nguyên văn |
|---|---|---|---|---|---|---|---|

## 4. LUỒNG CHÍNH (HAPPY PATH)
Khách hàng đăng nhập tài khoản -> Thêm sản phẩm vào giỏ -> Vào trang Thanh toán -> Nhập mã vào ô "Mã giảm giá" và bấm nút "Áp dụng" -> Hệ thống kiểm tra điều kiện mã hợp lệ -> Hiển thị số tiền được giảm và tổng tiền mới -> Bấm Đặt hàng thành công. (Nguồn: INPUT/Spec Function D.md §1, gate_status.json)

## 5. LUỒNG PHỤ & LỖI (ALTERNATE & ERROR FLOWS)

| ID | Tình huống | Hệ thống phản hồi (nguyên văn nếu có) | Nguồn (file §mục) | Trích nguyên văn |
|---|---|---|---|---|
| ALT-01 | Mã không hợp lệ hoặc hết hạn | Hiển thị thông báo lỗi ngay dưới ô nhập | INPUT/Spec Function D.md §1, gate_status.json Q-D13-01, Q-D13-02 | Mã không hợp lệ hoặc hết hạn → hiển thị thông báo lỗi. |
| ALT-02 | Đơn chưa đạt giá trị tối thiểu | Báo lỗi đơn hàng chưa đủ giá trị tối thiểu quy định | gate_status.json Q-D03-01, Q-D13-01 | Chưa đủ đơn tối thiểu |
| ALT-03 | Mã hết lượt dùng toàn hệ thống | Báo lỗi mã đã hết lượt sử dụng | gate_status.json Q-D05-01, Q-D13-01 | Hết lượt dùng |
| ALT-04 | Khách hàng đã từng dùng mã này | Báo lỗi vượt quá giới hạn 1 lần dùng/tài khoản | gate_status.json Q-D06-01 | Tối đa 1 lần duy nhất cho mỗi tài khoản khách hàng |

## 6. NGOÀI PHẠM VI (OUT OF SCOPE)
> Chỉ những gì nguồn **nói rõ** là không làm, kèm trích nguồn. Điều tài liệu không nhắc tới là câu hỏi (Mục 7), không phải ngoài phạm vi.

| Nội dung loại trừ | Nguồn (file §mục) | Trích nguyên văn |
|---|---|---|
| Back-office admin chi tiết | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §8 | Back-office admin chi tiết |
| Tích hợp ERP/kho | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §8 | Tích hợp ERP/kho |
| Load test (hiệu năng tải cao) | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §8 | Load test (hiệu năng tải cao) |
| Native mobile app | INPUT/Hệ thống Thương mại điện tử “ShopGo”.md §8 | Native mobile app |
| Mua hàng và giao hàng sang Nhật Bản, chuyển đổi ngoại tệ | gate_status.json Q-X01-01 | Không hỗ trợ, chỉ phục vụ khách mua hàng và giao hàng tại Việt Nam với đơn vị tiền tệ VNĐ |
| Tiền tệ và múi giờ Nhật Bản cho hiệu lực mã | gate_status.json Q-X01-02 | Không áp dụng tiền tệ, múi giờ Nhật Bản vì chỉ bán tại Việt Nam |

## 7. CÂU HỎI MỞ & KẼ HỞ 06W (OPEN QUESTIONS & MISSING RULES)
> Skill `02` thêm dòng (`New`). Cột `Phản hồi chính thức`, `Người trả lời`, `Ngày`, `Trạng thái` do **con người** điền
> (hoặc agent chép nguyên văn khi người dùng chuyển lời BA). Trạng thái: `New` → `TREO` → `Confirmed` / `Rejected` (§9.3).
> Mã `MR-xx` cấp tiếp số lớn nhất đang có — không đánh lại từ 01.

| ID | Kẽ hở | Nhóm 06W | Nguồn phát hiện | Phương án gợi ý (chưa phải rule) | Câu hỏi cho BA/PO | Phản hồi chính thức | Người trả lời | Ngày | Trạng thái |
|---|---|---|---|---|---|---|---|---|---|
| M01 | Tiền giảm vượt tạm tính và phí ship | How | Q-D12-01, Q-D20-01 | Quy định trần theo tạm tính hay tổng gồm ship | Phần giá trị mã còn dư có được trừ tiếp vào phí ship không, trần là tạm tính hay tổng gồm ship? | <điền> | <điền> | <điền> | TREO |
| M02 | Thời điểm giữ lượt dùng | When | Q-D14-01, Q-D18-01 | Thống nhất tạm giữ khi Áp dụng hay trừ lúc ghi nhận Đặt hàng | Nếu lượt cuối đã tạm giữ khi Áp dụng, khách thứ hai có thể áp mã tranh chấp như Q-D18-01 không? | <điền> | <điền> | <điền> | TREO |
| M03 | Điều kiện ngoài phương án ở Q-D01-01 | What | Q-D01-01, Q-D03-01, Q-D03-02, BR-02 | Làm rõ ngưỡng áp dụng 200.000đ | Hóa đơn 200k tính trên tạm tính hay tổng ship, và có bị BR-02 ghi đè không? | <điền> | <điền> | <điền> | TREO |
| M04 | Cơ sở tính tiền giảm % | How | Q-D02-01, Q-D20-01, Q-D11-01 | Thống nhất cơ sở % tiền hàng hay tổng gồm ship | Tiền giảm % tính trên tạm tính hay trên tổng có phí ship? | <điền> | <điền> | <điền> | TREO |
| M05 | Phạm vi áp dụng của mã | Where | Q-D08-01, Q-D08-02 | Xác nhận hệ thống có loại mã giới hạn danh mục hay không | Có tồn tại mã giới hạn phạm vi sản phẩm hay mọi mã đều áp dụng toàn giỏ hàng? | <điền> | <điền> | <điền> | TREO |
| M06 | Toàn vẹn tiền giảm và rủi ro bảo mật | How | Q-D21-02, Q-D21-01, Q-D02-01, Q-D11-01, Q-D12-01 | Hệ thống tính toán độc lập tại server | Tiền giảm dùng giá trị giao diện gửi lên hay backend tính toán lại? | <điền> | <điền> | <điền> | TREO |
| M07 | Lượt tạm giữ khi mất phiên hoặc gỡ mã | When | Q-D16-03, Q-D14-01, Q-D06-01, Q-D05-01, Q-D17-01, Q-D09-02, Q-D16-01 | Tự động hoàn nhả lượt tạm giữ | Lượt đã tạm giữ có được nhả khi phiên hết hạn, khi gỡ mã hoặc mã bị ghi đè không? | <điền> | <điền> | <điền> | TREO |

## 8. QUYẾT ĐỊNH ĐÃ CHỐT (CONFIRMED DECISIONS LOG)
> Chỉ ghi khi BA/PO đã quyết. Thiếu `Người phê duyệt` hoặc `Ngày chốt` thì tool coi là chưa chốt.
> Lần chạy sau agent áp dụng luôn, không hỏi lại.

| # | Mã liên quan | Câu hỏi | Quyết định chính thức | Người phê duyệt | Ngày chốt |
|---|---|---|---|---|---|
| 1 | Q-D01-01 | Ngoài 2 loại mã giảm giá tiền hàng (% và số tiền cố định), hệ thống có hỗ trợ loại mã miễn phí hoặc giảm phí vận chuyển (Freeship) tại ô nhập này không? | A. Có hỗ trợ mã Freeship (nhưng mặc định áp dụng cho hóa đơn từ 200.000đ) | QA/BA | 2026-11-09 |
| 2 | Q-D02-01 | Mã giảm giá theo tỷ lệ phần trăm (%) có áp dụng mức giảm tối đa (trần giảm) không? | A. Có trần giảm tối đa (cấu hình theo từng mã cụ thể) | QA/BA | 2026-11-09 |
| 3 | Q-D03-01 | Giá trị đơn hàng tối thiểu quy định của mã được so sánh dựa trên cơ sở nào? | B. Tổng tiền đơn hàng (đã bao gồm phí vận chuyển) | QA/BA | 2026-11-09 |
| 4 | Q-D03-02 | Điều kiện "đạt giá trị tối thiểu quy định" được hiểu theo toán tử so sánh nào? | A. Lớn hơn hoặc bằng (>= giá trị tối thiểu quy định) | QA/BA | 2026-11-09 |
| 5 | Q-D04-01 | Ngày hết hạn của mã giảm giá được tính hiệu lực đến thời điểm nào trong ngày? | A. Đến 23:59:59 của ngày hết hạn | QA/BA | 2026-11-09 |
| 6 | Q-D04-02 | Múi giờ chuẩn được hệ thống sử dụng để kiểm tra ngày giờ hết hạn của mã là gì? | A. Múi giờ Việt Nam (GMT+7) | QA/BA | 2026-11-09 |
| 7 | Q-D05-01 | Hệ thống có giới hạn tổng số lượt sử dụng tối đa trên toàn sàn đối với một mã giảm giá không? | A. Có giới hạn tổng lượt dùng toàn hệ thống (hết lượt thì không dùng được nữa) | QA/BA | 2026-11-09 |
| 8 | Q-D06-01 | Một tài khoản khách hàng được phép áp dụng thành công một mã giảm giá tối đa bao nhiêu lần? | A. Tối đa 1 lần duy nhất cho mỗi tài khoản khách hàng | QA/BA | 2026-11-09 |
| 9 | Q-D07-01 | Khách vãng lai (Guest) có được nhập và áp dụng mã giảm giá tại trang Thanh toán trước khi đăng nhập không? | A. Bắt buộc phải đăng nhập tài khoản trước mới được nhập và áp dụng mã | QA/BA | 2026-11-09 |
| 10 | Q-D08-01 | Phạm vi áp dụng của mã giảm giá có giới hạn theo danh mục/sản phẩm cụ thể không? | A. Áp dụng cho toàn bộ sản phẩm trong giỏ hàng | QA/BA | 2026-11-09 |
| 11 | Q-D09-01 | Một đơn hàng được phép áp dụng đồng thời tối đa bao nhiêu mã giảm giá? | A. Tối đa 1 mã giảm giá duy nhất trên một đơn hàng | QA/BA | 2026-11-09 |
| 12 | Q-D09-02 | Khi đã áp dụng thành công mã A, nếu khách hàng nhập tiếp mã B và bấm Áp dụng thì hệ thống xử lý thế nào? | A. Tự động ghi đè mã mới thay thế mã cũ nếu mã mới hợp lệ | QA/BA | 2026-11-09 |
| 13 | Q-D10-01 | Ký tự của mã giảm giá khi nhập vào có phân biệt chữ hoa và chữ thường (Case-sensitive) không? | A. Không phân biệt hoa/thường (hệ thống tự động chuyển đổi thành chữ in hoa) | QA/BA | 2026-11-09 |
| 14 | Q-D10-02 | Khi người dùng nhập hoặc dán mã có khoảng trắng thừa ở đầu hoặc cuối chuỗi, hệ thống xử lý thế nào? | A. Tự động cắt bỏ khoảng trắng thừa (auto-trim) trước khi kiểm tra mã | QA/BA | 2026-11-09 |
| 15 | Q-D11-01 | Khi mã giảm theo tỷ lệ % sinh ra số tiền giảm có phần lẻ, hệ thống áp dụng quy tắc làm tròn nào cho tiền tệ VND? | C. Làm tròn số học đến hàng nghìn 1.000đ | QA/BA | 2026-11-09 |
| 16 | Q-D12-01 | Đối với mã giảm số tiền cố định, nếu giá trị giảm lớn hơn tạm tính tiền hàng của giỏ hàng thì hệ thống xử lý thế nào? | A. Tiền giảm tối đa bằng tạm tính tiền hàng (tiền hàng sau giảm về 0đ) | QA/BA | 2026-11-09 |
| 17 | Q-D13-01 | Khi áp dụng mã không thành công, hệ thống hiển thị thông báo lỗi chung hay tách biệt theo từng nguyên nhân cụ thể? | A. Tách biệt thông báo lỗi theo nguyên nhân (Hết hạn, Chưa đủ đơn tối thiểu, Hết lượt dùng, Mã không tồn tại) | QA/BA | 2026-11-09 |
| 18 | Q-D14-01 | Lượt sử dụng của mã giảm giá được hệ thống trừ chính thức vào thời điểm nào? | B. Tạm giữ lượt khi bấm Áp dụng mã thành công và trừ chính thức khi Đặt hàng thành công | QA/BA | 2026-11-09 |
| 19 | Q-D15-01 | Khi đơn hàng có áp dụng mã bị khách hàng hủy hoặc thanh toán trực tuyến qua thẻ thất bại, lượt dùng mã có được hoàn lại không? | A. Tự động hoàn lại lượt dùng mã cho khách hàng | QA/BA | 2026-11-09 |
| 20 | Q-D16-01 | Sau khi áp mã thành công, nếu khách hàng quay lại giỏ hàng sửa số lượng làm giá trị đơn tụt xuống dưới mức đơn tối thiểu thì hệ thống xử lý thế nào? | A. Tự động hủy áp dụng mã và thông báo đơn hàng không còn đủ điều kiện hưởng ưu đãi | QA/BA | 2026-11-09 |
| 21 | Q-D17-01 | Sau khi áp dụng mã thành công, khách hàng có thể chủ động gỡ bỏ mã giảm giá khỏi đơn hàng không? | A. Có nút hoặc biểu tượng gỡ mã, khi bấm vào thì gỡ mã và tính lại tổng tiền ban đầu | QA/BA | 2026-11-09 |
| 22 | Q-D18-01 | Khi mã chỉ còn 1 lượt dùng duy nhất và có 2 khách hàng cùng bấm Đặt hàng tại cùng một thời điểm, hệ thống xử lý thế nào? | A. Đơn hàng nào ghi nhận trước tại máy chủ sẽ áp dụng thành công, đơn hàng sau báo lỗi mã đã hết lượt | QA/BA | 2026-11-09 |
| 23 | Q-D19-01 | Nếu khách hàng đã áp mã thành công tại trang Thanh toán nhưng trước khi bấm Đặt hàng mà mã bị hết hạn hoặc bị Admin vô hiệu hóa, hệ thống xử lý thế nào? | A. Kiểm tra lại tại thời điểm bấm Đặt hàng, từ chối tạo đơn và thông báo mã không còn hiệu lực | QA/BA | 2026-11-09 |
| 24 | Q-D20-01 | Tiền giảm giá của mã (cả % và số tiền cố định) được giảm trừ vào thành phần nào của đơn hàng? | B. Giảm trừ vào tổng giá trị đơn hàng (bao gồm cả tiền hàng và phí vận chuyển) | QA/BA | 2026-11-09 |
| 25 | Q-D21-01 | Hệ thống có cơ chế giới hạn số lần nhập mã sai liên tiếp (Rate limit / Brute-force protection) để ngăn chặn bot dò mã không? | B. Không giới hạn số lần nhập mã sai | QA/BA | 2026-11-09 |
| 26 | Q-D22-01 | Khi khách hàng bấm nút "Áp dụng", hệ thống có hiển thị trạng thái chờ (loading) và vô hiệu hóa nút để chặn thao tác bấm đúp liên tục không? | A. Có hiển thị spinner/loading và vô hiệu hóa nút Áp dụng trong khi chờ API phản hồi | QA/BA | 2026-11-09 |
| 27 | Q-D23-01 | Sau khi đặt hàng thành công, thông tin mã giảm giá và số tiền được giảm có được lưu trữ và hiển thị trong chi tiết đơn hàng cùng email xác nhận đơn không? | A. Có lưu vết đầy đủ mã giảm giá, số tiền được giảm và hiển thị trong chi tiết đơn hàng, lịch sử đơn và email xác nhận | QA/BA | 2026-11-09 |
| 28 | Q-X01-01 | Tệp chat.md ghi nhận thông tin Leader nói có thể mua hàng ở Nhật Bản, trong khi ShopGo.md quy định người dùng cuối tại Việt Nam, tiền tệ VNĐ. Hệ thống có hỗ trợ mua và giao hàng sang Nhật Bản không? | A. Không hỗ trợ, chỉ phục vụ khách mua hàng và giao hàng tại Việt Nam với đơn vị tiền tệ VNĐ | QA/BA | 2026-11-09 |
| 29 | Q-D08-02 | Nếu mã giới hạn phạm vi và giỏ có cả sản phẩm thuộc và không thuộc phạm vi thì tiền giảm tính thế nào? | C. Không cho áp mã | BA (demo lớp học) | 2026-11-14 |
| 30 | Q-D13-02 | Thông báo lỗi hiển thị ở đâu? | A. Ngay dưới ô "Mã giảm giá" | BA (demo lớp học) | 2026-11-14 |
| 31 | Q-D15-02 | Khi thanh toán thẻ/Ví ShopGo thất bại thì lượt dùng mã xử lý thế nào? | A. Hoàn lượt, khách áp lại được | BA (demo lớp học) | 2026-11-14 |
| 32 | Q-D16-02 | Sau khi áp mã %, khách đổi số lượng sản phẩm (vẫn đạt ngưỡng) thì tiền giảm có được tính lại không? | A. Tự tính lại theo giỏ mới | BA (demo lớp học) | 2026-11-14 |
| 33 | Q-D16-03 | Khi phiên đăng nhập hết hạn sau khi đã áp mã, đăng nhập lại thì mã có được giữ không? | B. Mất mã, khách phải áp lại | BA (demo lớp học) | 2026-11-14 |
| 34 | Q-D20-02 | Khi đổi địa chỉ giao làm thay đổi phí ship sau khi đã áp mã thì mã có được kiểm tra lại không? | A. Có, kiểm tra và tính lại | BA (demo lớp học) | 2026-11-14 |
| 35 | Q-D21-02 | Tiền giảm được tính và xác nhận ở đâu? | B. Dùng giá trị giao diện gửi lên | BA (demo lớp học) | 2026-11-14 |
| 36 | Q-X01-02 | Nếu khách tại Nhật Bản được mua hàng, tiền tệ và múi giờ hiệu lực mã áp dụng thế nào? | C. Không áp dụng vì chỉ bán tại Việt Nam | BA (demo lớp học) | 2026-11-14 |

## 9. HẰNG SỐ NGHIỆP VỤ (DOMAIN CONSTANTS)
> Chỉ hằng số có trong nguồn. Dùng cho chuỗi biên và sinh dữ liệu test.

| Hằng số | Kiểu dữ liệu | Giá trị | Phạm vi ảnh hưởng | Nguồn (file §mục) |
|---|---|---|---|---|

## 10. MA TRẬN TRUY VẾT (TRACEABILITY MATRIX)

| Mã | Nguồn (file §mục) | Test Case IDs liên quan | Ghi chú |
|---|---|---|---|
