# Bộ tài liệu kỹ thuật — Personal Finance Manager

| Tài liệu | Mục đích |
|---|---|
| [Kế hoạch sản phẩm](personal-finance-wallet-plan.md) | Định vị, SWOT, roadmap và phạm vi MVP |
| [BRD & SRS](ba-srs.md) | Yêu cầu nghiệp vụ, use case, quy tắc và tiêu chí chấp nhận |
| [Domain, dữ liệu & API](domain-data-api.md) | Ubiquitous language, mô hình dữ liệu, đồng bộ và OpenAPI outline |
| [Kiến trúc, UX & chất lượng](architecture-ux-quality.md) | Kiến trúc, NFR, security/privacy, UX/service blueprint, test và vận hành |

## Ranh giới sản phẩm

Đây là **Personal Finance Manager (PFM) tracking-only**: ghi nhận/hiển thị dữ liệu tài chính do người dùng cung cấp. Ứng dụng không giữ tiền, không khởi tạo thanh toán, không đặt lệnh đầu tư, không thu thông tin đăng nhập/OTP ngân hàng và không đưa khuyến nghị đầu tư cá nhân hóa.

Nếu chuyển sang kết nối ngân hàng hoặc thực hiện giao dịch, đó là một chương trình riêng: pháp lý, đối tác được cấp phép, KYC/AML, data licensing, đối soát và threat model phải được thẩm định lại trước khi thiết kế.
