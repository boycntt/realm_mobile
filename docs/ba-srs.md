# BRD & SRS — Quản lý thu chi, tiền gửi, đầu tư và tích lũy

> Phiên bản 1.0 · Thị trường: Việt Nam · Loại sản phẩm: PFM tracking-only

## 1. Mục tiêu, phạm vi và nguyên tắc

**Mục tiêu kinh doanh.** Giúp người dùng có bức tranh dòng tiền và tài sản đáng tin: biết tiền khả dụng đến kỳ lương, kiểm soát ngân sách, theo dõi mục tiêu và thấy được giá trị ước tính của tiền gửi/danh mục.

**Không thuộc phạm vi.** Nhận/giữ/chuyển tiền; mở/tất toán tiền gửi; đặt/sửa/hủy lệnh chứng khoán/quỹ; khuyến nghị đầu tư cá nhân; thu OTP, mật khẩu ngân hàng hoặc private key. Các tính năng này không được suy ra từ wording/UI.

**Nguyên tắc BA.** Tin cậy trước, tự động sau; dữ liệu nguồn và thời điểm luôn hiển thị; forecast không lẫn với số đã xác nhận; sửa số liệu không được làm mất lịch sử đã post.

## 2. Personas và Jobs-to-be-done

| Persona | Job | Nỗi đau | Thành công |
|---|---|---|---|
| Minh, 24, nhân viên mới | Kiểm soát chi tiêu đến kỳ lương | Nhiều ví, chi nhỏ, ngại nhập | Ghi chi trong <10 giây, biết số tiền có thể chi |
| Lan, 32, gia đình trẻ | Lập ngân sách gia đình | Hóa đơn/chi chung khó nhìn | Xem giới hạn từng nhóm và khoản sắp đến hạn |
| Huy, 29, freelancer | Tách tiền đã nhận với phải thu | Dòng tiền biến động | Xem cash runway mà không nhầm khoản pending là tiền thật |
| Thu, 39, kỷ luật tài chính | Đối soát và theo dõi tài sản | Sợ số liệu sai/trùng | Có provenance, export và reconciliation rõ ràng |

## 3. Ubiquitous language

| Thuật ngữ | Định nghĩa chuẩn | Không được nhầm với |
|---|---|---|
| Available cash | Tiền đã post trong các tài khoản thanh khoản, sau các điều chỉnh đã biết | Net worth hoặc hạn mức thẻ |
| Transaction | Sự kiện kinh tế gồm một hay nhiều ledger entry | Một dòng giao diện |
| Transfer | Chuyển giá trị giữa hai account của cùng financial space | Thu nhập/chi tiêu |
| Goal | Mục tiêu tài chính theo dõi | Sản phẩm tiết kiệm hoặc tài khoản phong tỏa |
| Term deposit | Khoản tiền gửi do người dùng khai báo | Sổ do app mở/đảm bảo |
| Holding | Vị thế tài sản đang nắm giữ | Giá trị thị trường realtime |
| Valuation | Giá trị ước tính có nguồn và thời điểm | Giá thanh lý hoặc cam kết lợi nhuận |
| Posted / pending | Đã ảnh hưởng số dư / chỉ dự báo | Đã đối soát |

## 4. Yêu cầu chức năng — Thu chi

### FR-CF-01. Accounts và số dư mở đầu

- Hỗ trợ cash, bank, e-wallet, debit card, credit card, loan/other liability.
- Bắt buộc tên, loại, tiền tệ; số dư mở đầu/ngày hiệu lực là tùy chọn. Với thẻ tín dụng dùng thuật ngữ dư nợ/hạn mức, không gọi chung là số dư.
- Không được hard-delete account có giao dịch: archive hoặc yêu cầu remap. Thay đổi opening balance sau khi có giao dịch phải nêu tác động và ghi audit.

### FR-CF-02. Thu, chi, chuyển, hoàn tiền và điều chỉnh

- Tạo expense/income với account, amount > 0, business date, category hợp lệ; note, tag, attachment tùy chọn.
- Transfer yêu cầu account nguồn ≠ account đích; tạo hai legs nguyên tử. Phí là leg expense riêng. Transfer principal và credit-card payment không được tính hai lần vào chi tiêu/ngân sách.
- Refund có thể link giao dịch gốc. Chỉ làm giảm chi ngân sách khi user xác nhận/link đúng; nếu không, hạch toán income.
- Adjustment yêu cầu lý do, actor và audit; không dùng thay cho giao dịch thường ngày.
- Có split giao dịch: tổng split phải đúng principal, category tương thích loại giao dịch. Báo cáo/budget dùng split thay vì category header mơ hồ.

### FR-CF-03. Trạng thái, định kỳ và đối soát

`draft → pending → posted → void/reversed`. Chỉ `posted` tác động actual balance/budget. Pending income chỉ nằm forecast.

- Recurrence có template, tần suất, ngày bắt đầu/kết thúc, `remind` hoặc `auto-post`, occurrence key duy nhất. Nếu ngày 31 không có trong tháng, chính sách mặc định là ngày cuối tháng.
- Reconciliation lưu opening/closing balance, sao kê period, chênh lệch và giao dịch included. Account chỉ được gọi “đã đối soát” sau session completed.
- Với nhập CSV/dữ liệu đối tác, nghi trùng chỉ là đề xuất. Người dùng chọn Import/Skip/Merge; không tự xóa hay merge.

### FR-CF-04. Budget, dashboard và báo cáo

- Budget có scope (personal/household), kỳ, account scope, danh mục, limit, rollover và thresholds. Budget chồng nhau phải hiển thị là tổng/con, không cộng limit vô căn cứ.
- Dashboard tách: total assets, available cash, card liability, forecast và net worth. Mỗi số phải drill-down được tới nguồn.
- Báo cáo lọc theo kỳ, account, category/tag và chỉ rõ timezone/currency; export CSV/JSON có confirmation và audit event.

## 5. Yêu cầu chức năng — Tiền gửi, đầu tư, tích lũy

### FR-INV-01. Tiền gửi (tracking)

- Lưu tổ chức, amount gốc, currency, ngày mở/đáo hạn, kỳ hạn, lãi suất năm, day-count basis, tần suất trả lãi, cách tái tục, lãi rút trước hạn (nếu biết).
- Hiển thị riêng: gốc, lãi đã nhận, lãi ước tính, tiền dự kiến đáo hạn. Nhãn bắt buộc: “ước tính theo dữ liệu bạn nhập; tổ chức phát hành là nguồn quyết toán”.
- State: `active`, `matured_unconfirmed`, `closed`, `renewed`. Không tự tái tục hay tự post lãi khi đáo hạn. Tái tục tạo hợp đồng/lô mới, liên kết hợp đồng cũ.
- Reminder opt-in trước 30/7/1 ngày; không hiển thị số tiền trên lock screen mặc định.

### FR-INV-02. Investment holdings (tracking)

- Các asset type: bond, fund_certificate, stock, ETF, gold, crypto, other. Người dùng khai báo giao dịch buy/sell/dividend/coupon/fee/tax/split/adjustment.
- Holding cần quantity, cost basis, fee/tax, trade date, settle date, original currency. Sell không được làm âm quantity ở account không cho short.
- Quote/FX luôn có `source`, `quotedAt`, `receivedAt`, currency và cờ stale/missing. Không có giá thì không giả định bằng 1 hoặc gọi dữ liệu realtime.
- P&L thực hiện/chưa thực hiện tách riêng. Chính sách cost basis (FIFO hoặc weighted average) là cấu hình cố định trên account/reporting period.

### FR-INV-03. Savings goals

- Goal có target amount/date, monthly contribution, priority và mode `reserved_cash` hoặc `invested_toward_goal`.
- Progress chỉ dùng confirmed allocation/contribution; planned contribution là forecast riêng. Tổng allocation của một holding không vượt 100%.
- Kịch bản sinh lời là opt-in, phải hiện giả định/rủi ro, không là lời hứa hay khuyến nghị.

## 6. Quy tắc nghiệp vụ và công thức

1. Money lưu integer minor units/fixed decimal, tuyệt đối không `float`; VND hiển thị nguyên. Timestamp lưu UTC kèm timezone IANA tại thời điểm nghiệp vụ.
2. `available_balance = opening_balance + posted ledger entries`; ngày tương lai/pending không cộng actual.
3. Transfer principal có tổng tác động toàn financial space bằng 0; phí làm giảm net worth.
4. `market_value = Σ(remaining_quantity × latest_eligible_price × FX_to_base)`. Quote hợp lệ phải đúng currency, trước cutoff và chưa vượt `max_staleness` theo asset type.
5. `unrealized_pnl = market_value - remaining_cost_basis`; thiếu/stale price hoặc FX phải làm tổng có data-quality flag và completeness ratio.
6. Lãi tiền gửi dự kiến dùng day-count/tần suất được khai báo, không hard-code 365. Cashflow lãi chỉ post vào ngày payment khi user xác nhận/import.
7. `required_monthly = max(0, (target - confirmed_amount) / remaining_months)`; quá hạn target date thì trả thiếu hụt, không chia cho 0.
8. Sửa bản ghi posted tạo reversal/adjustment hoặc phiên bản audit, không overwrite lịch sử đã đối soát.

## 7. Use cases và acceptance criteria trọng yếu

| Use case | Luồng thành công | Acceptance criteria then |
|---|---|---|
| UC-CF-01 Ghi chi nhanh | Quick add → amount → xác nhận | Default account/category gần nhất; invalid amount/account/category không post; undo tạo audit-safe reversal |
| UC-CF-02 Chuyển tiền | Source → destination → review → post | Tạo đúng 2 legs; dashboard không tăng chi; retry không duplicate; fee tách rõ |
| UC-CF-03 Budget | Chọn category/kỳ/limit | Chỉ expense posted vào spend; 80/100/120% configurable; warning có remaining + days left |
| UC-CF-04 Import & merge | Map file → preview → resolve duplicate → import | Partial import có summary; record nghi trùng không tự merge; batch idempotent |
| UC-CF-05 Reconciliation | Nhập closing balance → review difference → complete | Công thức chênh lệch rõ; sửa sau completion tạo exception |
| UC-INV-01 Tạo term deposit | Nhập gốc/rate/dates → preview → save | Maturity > start; rate/amount không âm; không phát sinh lệnh hay cashflow thật |
| UC-INV-02 Nhập buy/sell | Nhập trade/quantity/price/fee | Không oversell nếu no-short; confirmed edit tạo adjustment; P&L tính lại nhất quán |
| UC-INV-03 Valuation | Xem portfolio base VND | Mỗi total có as-of, source, coverage, quote/FX status; drill-down tới amount gốc |
| UC-INV-04 Goal | Tạo target/contribution | Progress chỉ confirmed; forecast nêu assumption; allocation vượt 100% bị chặn |

## 8. Luồng ngoại lệ

| Điều kiện | Hành vi bắt buộc |
|---|---|
| Offline khi post | Local save + outbox; UI “đã lưu trên thiết bị”; sync retry idempotent |
| Hai thiết bị cùng sửa | Optimistic concurrency; hiển thị before/after conflict, không silent overwrite |
| Category/account archive | Giữ lịch sử, chặn chọn mới, yêu cầu mapping |
| Amount/CSV format mơ hồ | Preview normalized delimiter/decimal và xác nhận parser |
| Quote hoặc FX stale/missing | Gắn cờ, giảm completeness, không bịa tỷ giá/return |
| Maturity chưa xác nhận | `matured_unconfirmed`; chỉ nhắc việc, không auto-renew |
| Đổi timezone | Giữ UTC + original timezone; reporting theo policy profile có công khai |

## 9. KPI và ưu tiên backlog

North-star: tỷ lệ người dùng có **bức tranh dòng tiền đáng tin** mỗi tháng (≥80% giao dịch posted được category hoặc reconciliation, và chủ động xem ít nhất một budget/insight).

P0: account, income/expense/transfer/split, budget, dashboard, local encryption/app lock, backup/export, money/ledger tests, term deposit, goals, manual holdings + valuation provenance. P1: CSV import, duplicate/reconciliation, smart category suggestion có xác nhận, multi-currency, price import. P2: partner integration/market data/AI chỉ sau legal-security-data licensing gate.
