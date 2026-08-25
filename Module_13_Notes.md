# 📚 CEH v13 Study Notes - Module 13: Hacking Web Servers

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Trong CEH v13, kẻ tấn công dùng GenAI để phân tích kiến trúc Web Server cực nhanh, tự động hóa việc tìm lỗ hổng. Nhưng nếu bạn không hiểu Web Server (Apache, IIS) chạy dịch vụ gì, lưu log ở đâu, AI cũng không thể gánh vác cho bạn."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân loại Kiến trúc Web (Web Server vs Web Application). Đề thi luôn kiểm tra xem bạn có phân biệt được lỗi do *Cấu hình máy chủ* hay lỗi do *Code của lập trình viên* hay không."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Cách thức tấn công trực tiếp vào máy chủ (Web Server) lưu trữ trang web. Mục tiêu là khai thác các lỗ hổng hệ điều hành, lỗi phần mềm Web Server (IIS, Apache, Nginx) hoặc lỗi cấu hình mạng.
*   **Tại sao quan trọng:** Máy chủ Web là "bộ mặt" của doanh nghiệp, luôn luôn phải kết nối với Internet (Port 80/443). Nếu Hacker hack được Web Server, chúng có thể thay đổi giao diện (Defacement), ăn cắp cơ sở dữ liệu, hoặc biến máy chủ đó thành "trạm trung chuyển" (Pivot) để tấn công sâu vào mạng nội bộ công ty (Intranet).

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Web Server (Máy chủ Web):** Phần cứng hoặc phần mềm (Apache, IIS, Nginx) làm nhiệm vụ tiếp nhận yêu cầu HTTP từ trình duyệt và trả về nội dung (HTML, CSS, Images).
*   **Web Application (Ứng dụng Web):** Phần mềm chạy *trên* Web Server (viết bằng PHP, Java, .NET) xử lý logic nghiệp vụ. (Sẽ học ở Module 14).
*   **Website Defacement:** Hành vi thay đổi giao diện trang chủ của website (thường là để lại logo của Hacker hoặc thông điệp chính trị) sau khi chiếm quyền Web Server.
*   **Directory Traversal (Path Traversal):** Lỗ hổng cho phép kẻ tấn công vượt ra ngoài thư mục gốc (Root folder) của Web Server để truy cập các tệp tin nhạy cảm của hệ điều hành (như `/etc/passwd` trên Linux).
*   **Misconfiguration (Cấu hình sai):** Lỗi bảo mật phổ biến nhất. Ví dụ: Để nguyên mật khẩu mặc định, mở cổng quản trị Admin ra Internet, hiển thị chi tiết thông báo lỗi (Error messages).

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Directory Traversal** | Dùng chuỗi `../` để nhảy ra khỏi thư mục web, đọc tệp hệ thống. |
| 🔴 **MUST MEMORIZE** | **IIS / Apache / Nginx** | 3 phần mềm Web Server phổ biến nhất thế giới. Tấn công vào chúng là Hacking Web Servers. |
| 🔴 **MUST MEMORIZE** | **Website Defacement** | Đánh tráo giao diện web. Dấu hiệu rõ nhất cho thấy máy chủ đã bị hack. |
| 🟠 **HIGH PRIORITY** | **DMZ (Demilitarized Zone)** | Vùng đệm mạng. Web Server luôn phải đặt ở đây để bảo vệ mạng nội bộ. |
| 🟠 **HIGH PRIORITY** | **Banner Grabbing** | Kỹ thuật lấy phiên bản của Web Server (vd: Apache 2.4.49) để tìm mã khai thác. |
| 🟡 **SHOULD KNOW** | **HTTP Response Splitting** | Khai thác bộ lọc HTTP để nhét 2 phản hồi vào 1 yêu cầu (Cache Poisoning). |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Metasploit** | Exploitation Framework | Công cụ bách khoa toàn thư chứa hàng vạn mã khai thác (Exploit) dành cho các lỗ hổng Web Server đã biết. | Bắt buộc phải biết cách dùng. |
| **Nikto** | Web Server Scanner | Quét Web Server để tìm thư mục ẩn, lỗi cấu hình, phiên bản cũ, và file CGI nguy hiểm. | Cực kỳ ồn ào, dễ bị WAF chặn. |
| **Nmap** | Scanner | Dùng script `--script http-enum` hoặc bắt Banner của Server. | Khởi đầu của mọi cuộc tấn công. |
| **Wfetch / Netcat** | Banner Grabbing | Kết nối tay (manual) vào Port 80 và gửi lệnh HTTP GET để xem máy chủ trả về Header gì. | Công cụ thô sơ nhưng hiệu quả. |
| **AI-Assisted Scanners** | **AI-Powered** | Kẻ tấn công dùng AI để phân tích mã nguồn mở của Web Server (như Apache) nhằm tìm ra Zero-day, hoặc dùng LLM để viết mã khai thác tự động. | Xu hướng mới trong CEHv13. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> Lệnh Directory Traversal kinh điển xuất hiện trong mọi kỳ thi bảo mật.

*   **Directory Traversal (Linux):** `http://www.victim.com/show.asp?file=../../../../etc/passwd` (Nhảy lùi 4 thư mục để đọc tệp chứa user Linux).
*   **Directory Traversal (Windows):** `http://www.victim.com/show.asp?file=..\..\..\..\boot.ini` (Nhảy lùi để đọc file hệ thống Windows).
*   **Netcat Banner Grabbing:**
    ```bash
    nc www.victim.com 80
    HEAD / HTTP/1.0
    (Bấm Enter 2 lần)
    ```
    *Máy chủ sẽ trả về Header chứa thông tin OS và Phiên bản Web Server.*

## 6. ATTACKS & TECHNIQUES
### Phân loại Tấn Công Web Server:
1.  **Banner Grabbing / OS Fingerprinting:** Đọc HTTP Header để biết máy chủ chạy Windows (IIS) hay Linux (Apache). Từ đó Google tìm mã khai thác (Exploit).
2.  **Directory Traversal (Dot-Dot-Slash):** Lợi dụng việc Web Server không lọc ký tự `../`. Hacker trèo ra ngoài `C:\inetpub\wwwroot` để vào `C:\Windows\System32` đọc cấu hình.
3.  **Website Defacement:** Sau khi có quyền ghi (Write), Hacker ghi đè tệp `index.html` của trang chủ.
4.  **Misconfiguration Exploits:** Hacker tìm thấy thư mục `/admin` không cần mật khẩu, hoặc tìm thấy file Backup `.bak` chứa mật khẩu Database bị admin để quên trên Web Server.
5.  **HTTP Response Splitting:** Kẻ tấn công gửi các ký tự Carriage Return / Line Feed (`CRLF - %0d%0a`) vào header HTTP. Web Server bị lú lẫn và tách 1 yêu cầu thành 2 phản hồi, giúp Hacker đầu độc bộ nhớ đệm (Cache Poisoning).
6.  **Web Server DoS (Application DoS):** Dùng Slowloris hoặc RUDY đánh sập Web Server (Đã học ở Module 10).

## 7. PROTOCOLS/PORTS/SERVICES
*   **HTTP (Port 80) / HTTPS (Port 443):** Cửa ngõ bắt buộc phải mở.
*   **Vùng đệm (DMZ):** Web Server phải luôn được đặt trong DMZ. Nếu Web Server bị hack, Firewall số 2 sẽ ngăn chặn Hacker từ DMZ nhảy vào mạng nội bộ (Intranet/LAN).

## 8. IMPORTANT NUMBERS & FACTS
*   **HTTP Status Codes (Phải thuộc lòng):**
    *   **200 OK:** Truy cập thành công.
    *   **301/302 Redirect:** Chuyển hướng.
    *   **401 Unauthorized:** Yêu cầu xác thực.
    *   **403 Forbidden:** Bị cấm truy cập (Có thư mục nhưng không cho xem).
    *   **404 Not Found:** Không tìm thấy tệp.
    *   **500 Internal Server Error:** Lỗi máy chủ (Thường xuất hiện khi Hacker tiêm mã thành công làm sập logic web).

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Hacking Web Servers (Mod 13) | Hacking Web Apps (Mod 14) |
| :--- | :--- | :--- |
| **Mục tiêu tấn công** | Phần mềm Máy Chủ (IIS, Apache, Nginx) hoặc HĐH (Windows/Linux). | Code của lập trình viên viết ra (PHP, ASPX, Database). |
| **Loại lỗ hổng** | Lỗi cấu hình (Misconfiguration), Directory Traversal, Thiếu bản vá (Missing Patch). | SQL Injection, XSS, CSRF, Logic Flaws. |
| **Trách nhiệm bảo mật** | Network Admin / System Admin. | Software Developer (Coder). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Directory Traversal:** Đề bài: *"Kẻ tấn công thêm chuỗi `../../../../etc/passwd` vào thanh URL. Đây là tấn công gì?"* -> Chọn **Directory Traversal** hoặc **Path Traversal**.
*   **HTTP Response Splitting:** Hỏi: *"Kỹ thuật chèn ký tự CR/LF (`%0d%0a`) vào HTTP Header để điều khiển phản hồi của máy chủ là gì?"* -> Chọn **HTTP Response Splitting**.
*   **Bảo vệ Web Server:** Đề hỏi: *"Vị trí kiến trúc mạng tốt nhất để đặt Web Server là ở đâu?"* -> Chọn **DMZ (Demilitarized Zone)**.
*   **Defacement:** Hỏi: *"Hành động thay đổi nội dung trang chủ của một website mang tính khoe khoang hoặc chính trị gọi là gì?"* -> Chọn **Website Defacement**.
*   **Banner Grabbing:** Hỏi: *"Cách tốt nhất để biết website đang chạy Apache phiên bản nào?"* -> Chọn **Banner Grabbing** (Sử dụng Telnet hoặc Netcat).

## 11. COMMON CONFUSIONS
*   **Directory Traversal vs Local File Inclusion (LFI):**
    *   *Directory Traversal:* Chỉ **ĐỌC** tệp (Read only).
    *   *LFI (Module 14):* Đọc và **THỰC THI** tệp (Execute). Nếu tệp chứa mã PHP, server sẽ chạy mã đó.
*   **Misconfiguration vs Missing Patches:** Cấu hình sai (Misconfig) là do Admin lười/yếu kém (ví dụ quên đổi pass mặc định). Thiếu bản vá (Missing Patch) là lỗi của nhà sản xuất (Apache có bug), Admin chưa kịp cập nhật bản sửa lỗi.

## 12. REAL-WORLD CONTEXT
*   **IIS WebDAV Lỗ hổng kinh điển:** Trên máy chủ Windows cũ (IIS 6.0), tính năng WebDAV thường xuyên bị cấu hình sai, cho phép Hacker sử dụng lệnh `PUT` (thay vì `GET`) để upload trực tiếp một file Web Shell (`.asp`) lên máy chủ, sau đó chạy nó để chiếm quyền điều khiển.
*   **Log Poisoning:** Một kỹ thuật thực tế để nâng cấp từ Directory Traversal lên RCE (Remote Code Execution) là Hacker sẽ gửi 1 đoạn code PHP độc hại thông qua User-Agent. Đoạn code này được Web Server lưu vào tệp `/var/log/apache2/access.log`. Sau đó Hacker dùng Directory Traversal đọc tệp log này, kích hoạt mã độc thành công.

## 13. QUICK REVISION
1.  **Kỹ thuật chèn `../` để thoát khỏi thư mục gốc của Web Server gọi là gì?** -> Directory Traversal.
2.  **Hành động thay đổi giao diện trang chủ website của nạn nhân gọi là gì?** -> Website Defacement.
3.  **Công cụ nào chuyên dùng để quét lỗi cấu hình và thư mục ẩn của Web Server?** -> Nikto.
4.  **Web Server nên được đặt ở phân vùng mạng nào để bảo mật nhất?** -> DMZ.
5.  **Mã trạng thái HTTP nào báo hiệu "Lỗi máy chủ nội bộ" (Internal Server Error)?** -> 500.
6.  **Kỹ thuật lấy thông tin hệ điều hành và phiên bản Web Server qua HTTP Header gọi là gì?** -> Banner Grabbing.

## 14. MEMORY HOOKS
*   **Directory Traversal:** Kỹ thuật "Leo rào". Dùng `../` làm cái thang để trèo qua bức tường thư mục Web (wwwroot) nhảy vào nhà kho (System32) của máy chủ.
*   **DMZ (Vùng đệm):** Giống như phòng tiếp khách ngoài sân. Khách (Internet) có thể vào sân xem Web Server, nhưng bị khóa cửa không cho vào phòng ngủ (Intranet/LAN).
*   **Banner Grabbing:** Nhìn biển số xe (Banner) để biết xe sản xuất năm nào, hãng gì (Apache 2.4).
