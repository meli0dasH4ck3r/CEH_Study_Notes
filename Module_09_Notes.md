# 📚 CEH v13 Study Notes - Module 09: Social Engineering

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Trong CEH v13, AI (GenAI, Deepfake) biến Social Engineering thành vũ khí khủng khiếp nhất vì nó giải quyết rào cản ngôn ngữ và ngoại hình (tạo video sếp giả mạo ra lệnh chuyển tiền). Tuy vậy, các hình thức tâm lý cốt lõi (Lòng tham, Sự sợ hãi, Lòng thương hại) của con người vẫn không đổi."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân loại tấn công: Human-based (Dựa vào con người), Computer-based (Dựa vào máy tính) và Mobile-based. Đây là điểm mấu chốt để ăn điểm trong đề thi."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Nghệ thuật thao túng tâm lý con người để họ tự nguyện phá vỡ các quy tắc bảo mật. Thay vì tìm lỗ hổng trong Firewall hay hệ điều hành, Hacker "hack" vào bộ não con người (Human OS).
*   **Tại sao quan trọng:** Con người luôn là mắt xích yếu nhất trong hệ thống bảo mật. Một hệ thống mã hóa AES-256 bit trị giá hàng triệu đô la có thể bị phá vỡ chỉ bằng một cuộc gọi điện thoại giả danh hoặc một chiếc USB thả rơi ở bãi đỗ xe. Social Engineering không đòi hỏi kỹ năng lập trình, nhưng hiệu quả cực cao.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Social Engineering (Kỹ thuật Kỹ nghệ Xã hội):** Kỹ thuật thuyết phục, đe dọa, hoặc thao túng người khác để tiết lộ thông tin bí mật (mật khẩu, sơ đồ mạng) hoặc thực hiện một hành động có lợi cho kẻ tấn công (bấm vào link mã độc).
*   **Phishing (Lừa đảo qua mạng):** Gửi email hàng loạt giả mạo các tổ chức uy tín (Ngân hàng, PayPal) để lừa người dùng nhập thông tin đăng nhập.
*   **Impersonation (Giả danh):** Kẻ tấn công đóng giả một người có thẩm quyền (Giám đốc, Công an, Nhân viên IT) để ra lệnh cho nạn nhân.
*   **Baiting (Thả thính/Làm mồi):** Lợi dụng lòng tham hoặc sự tò mò của con người (VD: Vứt một cái USB có nhãn "Lương Giám Đốc 2023" ngoài bãi xe công ty).
*   **Tailgating / Piggybacking (Theo đuôi):** Người không có thẻ đi theo một nhân viên có thẻ hợp lệ để lọt vào cửa bảo mật mà không cần quẹt thẻ.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Spear Phishing** | Lừa đảo Phishing **nhắm mục tiêu cụ thể** (vào 1 cá nhân hoặc 1 công ty). Khó nhận diện hơn Phishing thường. |
| 🔴 **MUST MEMORIZE** | **Whaling** | Kỹ thuật Spear Phishing nhưng mục tiêu là **"Cá voi" (Cấp cao/Sếp/CEO)**. |
| 🔴 **MUST MEMORIZE** | **Tailgating / Piggybacking** | Kẻ xấu đi ké cửa qua trạm kiểm soát vật lý. Tailgate (đi lén lút) / Piggyback (có sự đồng thuận của nhân viên). |
| 🟠 **HIGH PRIORITY** | **Shoulder Surfing** | Đứng phía sau nhìn lén (mật khẩu, mã ATM). Không dùng kỹ thuật số. |
| 🟠 **HIGH PRIORITY** | **Dumpster Diving** | Lục thùng rác công ty để tìm các tài liệu nhạy cảm chưa bị xé vụn (Sơ đồ mạng, danh sách email). |
| 🟡 **SHOULD KNOW** | **Vishing / Smishing** | Vishing = Voice (Gọi điện lừa đảo). Smishing = SMS (Nhắn tin SMS lừa đảo). |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **SET (Social-Engineer Toolkit)** | SE Framework | Framework kinh điển viết bằng Python. Dễ dàng sao chép website (Site Cloner), gửi email Phishing hàng loạt, tạo payload USB. | Bắt buộc phải biết khi thực hành Kali Linux. |
| **Gophish** | Phishing Framework | Hệ thống quản lý và thống kê chiến dịch Phishing mã nguồn mở. Dành cho Red Team kiểm tra nhận thức nhân viên. | Giao diện Web cực đẹp và mạnh. |
| **Maltego** | OSINT Tool | Dùng để thu thập thông tin tình báo về mục tiêu (Facebook, LinkedIn) để chuẩn bị kịch bản Spear Phishing. | Dùng trong khâu chuẩn bị (Footprinting). |
| **ChatGPT / LLMs** | **AI-Powered** | Khắc phục điểm yếu chí mạng của Hacker nước ngoài: Viết email Phishing cực chuẩn ngữ pháp, văn phong tự nhiên. | Tạo ra Spear Phishing quy mô lớn. |
| **Deepfake / Voice Cloning** | **AI-Powered** | Clone giọng nói của Giám đốc (qua file ghi âm vài giây) hoặc khuôn mặt qua video để thực hiện Vishing / Video Call lừa đảo cấp cao. | Lỗ hổng tâm lý tàn phá nhất trong v13. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!NOTE]
> Social Engineering ít liên quan đến các dòng lệnh hack trực tiếp, nó chủ yếu là các kịch bản tương tác người dùng. Tuy nhiên, nếu bạn dùng Kali, bạn nên biết cách gọi công cụ:
*   `setoolkit`: Lệnh gọi chạy **Social-Engineer Toolkit** trên terminal của Kali Linux. Từ Menu, người ta thường chọn Option `1` (Social-Engineering Attacks) -> Option `2` (Website Attack Vectors) -> Option `3` (Credential Harvester Attack Method) để nhân bản trang đăng nhập của Facebook/Google.

## 6. ATTACKS & TECHNIQUES
### Phân loại các hình thức Social Engineering:
1.  **Human-Based (Dựa vào con người - Trực tiếp):**
    *   **Impersonation:** Giả dạng nhân viên sửa điện mạng, nhân viên vệ sinh.
    *   **Eavesdropping:** Nghe lén các cuộc hội thoại công việc tại quán cà phê.
    *   **Shoulder Surfing:** Nhìn trộm mật khẩu.
    *   **Dumpster Diving:** Lục thùng rác.
    *   **Tailgating / Piggybacking:** Đu bám qua cửa bảo vệ.
2.  **Computer-Based (Dựa vào máy tính):**
    *   **Phishing / Spear Phishing / Whaling:** Lừa đảo qua Email.
    *   **Baiting:** Thả mồi (USB nhiễm mã độc).
    *   **Watering Hole Attack:** Kẻ tấn công không đánh thẳng vào công ty bạn, mà đánh sập và chèn mã độc vào một trang Web mà nhân viên công ty bạn hay truy cập (VD: Trang diễn đàn chuyên ngành, trang gọi đồ ăn chung của tòa nhà).
3.  **Mobile-Based (Dựa vào thiết bị di động):**
    *   **Smishing:** Lừa đảo qua tin nhắn SMS (VD: Tin nhắn "Tài khoản ngân hàng bị khóa, click link...").
    *   **Vishing:** Voice Phishing. (VD: Gọi điện tự xưng là công an hoặc thuế).
    *   **Malicious Apps:** Ứng dụng giả mạo trên Store đòi cấp quyền danh bạ, tin nhắn.

## 7. PROTOCOLS/PORTS/SERVICES
*   **SMTP (Port 25):** Giao thức để truyền tải Email. Cấu hình DNS lỏng lẻo (thiếu DMARC, SPF, DKIM) sẽ cho phép Hacker **giả mạo (Spoofing)** địa chỉ người gửi (VD: gửi thư từ `admin@congty.com`).
*   **HTTP (Port 80):** Các link lừa đảo thường dùng HTTP thay vì HTTPS, nhưng ngày nay Hacker cũng mua chứng chỉ SSL (HTTPS) cho web giả để đánh lừa biểu tượng "ổ khóa" xanh của trình duyệt.

## 8. IMPORTANT NUMBERS & FACTS
*   Trong các đợt Red Teaming, tỷ lệ thành công của Social Engineering qua cổng vật lý (Tailgating / Đóng giả IT) thường lên đến trên 70%.
*   Rất nhiều vụ hack lớn nhất thế giới bắt đầu từ một nhân viên bị lừa qua Spear Phishing, hoặc một cuộc gọi điện thoại cho bộ phận Helpdesk yêu cầu "Reset mật khẩu" (Kỹ thuật Impersonation).

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Phishing | Spear Phishing | Whaling |
| :--- | :--- | :--- | :--- |
| **Mục tiêu** | Hàng nghìn/triệu người ngẫu nhiên. | Nhắm vào một cá nhân/bộ phận cụ thể. | Nhắm vào Cấp cao (CEO, CFO, CTO). |
| **Độ cá nhân hóa** | Rất thấp (Email chung chung "Dear Customer"). | Cao (Biết tên thật, vị trí công tác). | Cực cao (Dùng tin nhắn nội bộ, văn phong tài chính). |
| **Mục đích** | Lấy thẻ tín dụng, tài khoản mạng xã hội. | Trộm cắp dữ liệu công ty, cài mã độc nội bộ. | Đòi chuyển khoản tiền tỷ, chiếm quyền kiểm soát hệ thống lõi. |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Tailgating vs Piggybacking:** Đề thi rất hay bẫy điểm này.
    *   **Piggybacking:** Bạn có thẻ, bạn mở cửa, thấy 1 người bê thùng hàng to nhờ giữ cửa. Bạn có sự đồng thuận (consent) và cho người đó vào.
    *   **Tailgating:** Bạn quẹt thẻ đi vào, cửa đóng từ từ, kẻ xấu nhân lúc bạn không để ý (không có sự đồng thuận) đã len chân đi vào theo.
*   **Watering Hole Attack:** Đề bài: *"Nhóm APT không thể hack vào hệ thống ngân hàng A. Thay vào đó, họ chèn mã độc vào website của quán cà phê dưới sảnh tòa nhà mà nhân viên ngân hàng A hay truy cập."* -> Chọn **Watering Hole**.
*   **Các biện pháp bảo vệ:** Đề hỏi: *"Biện pháp tốt nhất chống lại Dumpster Diving?"* -> Chọn **Máy hủy tài liệu (Shredder / Burn documents)**. Đề hỏi: *"Chống lại Tailgating?"* -> Chọn **Cổng an ninh xoay (Mantrap / Turnstiles)** hoặc **Bảo vệ vật lý**.
*   **Phòng thủ Phishing toàn diện:** Giải pháp cốt lõi để chống Social Engineering là **Security Awareness Training (Đào tạo nhận thức an ninh cho nhân viên định kỳ)**.

## 11. COMMON CONFUSIONS
*   **Vishing vs Smishing:** Vishing là âm thanh (Voice), có yếu tố tương tác thời gian thực, có thể dùng AI Deepfake giọng nói. Smishing là dạng tin nhắn văn bản (SMS), tĩnh.
*   **Baiting vs Phishing:** Baiting thường liên quan đến một mồi nhử hấp dẫn (phim lậu miễn phí, nhạc lậu, chiếc USB xịn rơi ngoài sân). Phishing thường dùng các thông điệp cảnh báo sự sợ hãi (tài khoản hết hạn, hóa đơn chưa thanh toán).

## 12. REAL-WORLD CONTEXT
*   **Deepfake CEO Fraud:** Năm 2019 và 2024 đã ghi nhận nhiều trường hợp bộ phận tài chính (CFO) nhận được lệnh chuyển hàng chục triệu USD. Lý do: Họ tham gia một cuộc gọi Video Call nội bộ với "Sếp tổng" và các trưởng phòng khác, nhưng thực chất tất cả (hình ảnh + giọng nói) đều do AI sinh ra (AI Clone).
*   **USB Drop (Thả USB):** Dù rất cũ nhưng vẫn cực kỳ hiệu quả. Hacker ném 10 cái USB (có chứa Payload chạy tự động AutoRun) ở bãi xe hoặc khu vực lễ tân. Tâm lý con người luôn tò mò cắm vào máy tính công ty để xem bên trong có gì -> Mạng công ty bị nhiễm mã độc nội bộ. (Cách chống: Vô hiệu hóa tính năng AutoRun/AutoPlay trên Windows, chặn cắm USB mass storage).

## 13. QUICK REVISION
1.  **Kỹ thuật lục thùng rác để tìm mật khẩu ghi trên giấy gọi là gì?** -> Dumpster Diving.
2.  **Một người đi theo sau nhân viên có thẻ để lọt qua cổng bảo vệ mà không bị phát hiện là gì?** -> Tailgating.
3.  **Hành động gửi email lừa đảo nhắm riêng vào Tổng Giám đốc (CEO) gọi là gì?** -> Whaling.
4.  **Tấn công vào một website đối tác hoặc website mà nhân viên mục tiêu hay ghé thăm gọi là gì?** -> Watering Hole Attack.
5.  **Công cụ kinh điển viết bằng Python để nhân bản (clone) website lừa đảo?** -> SET (Social-Engineer Toolkit).
6.  **Cách tốt nhất để phòng chống các đòn tấn công Social Engineering là gì?** -> Đào tạo nhận thức nhân viên (User Awareness Training).

## 14. MEMORY HOOKS
*   **Vishing & Smishing:** **V** = **V**oice (Đàm thoại), **S** = **S**MS (Tin nhắn).
*   **Watering Hole (Hố nước):** Giống như con sư tử (Hacker) không đuổi theo con ngựa vằn (Mục tiêu), mà nó nằm phục ở hố nước (Trang web ưa thích) đợi con ngựa tới uống.
*   **Whaling:** Đi săn cá voi (Sếp lớn/CEO), không phải cá con (nhân viên).
