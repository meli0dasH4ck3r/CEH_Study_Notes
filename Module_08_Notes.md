# 📚 CEH v13 Study Notes - Module 08: Sniffing

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** CEH v13 nói về việc dùng AI để phân tích khổng lồ các tệp tin lưu lượng mạng (.pcap) nhằm tìm điểm bất thường. Nhưng nếu bạn không hiểu cấu trúc Header của giao thức TCP/IP hay nguyên lý ARP Poisoning, bạn sẽ không hiểu AI đang báo cáo điều gì."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** về nguyên lý hoạt động của ARP Spoofing và MAC Flooding. Đây là hai kỹ thuật nghe lén kinh điển nhất trong mạng LAN mà đề thi rất thích hỏi."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Kỹ thuật đánh chặn và "nghe lén" (Sniffing) lưu lượng mạng. Giảng dạy cách bắt các gói tin đang bay trên không (Wireless) hoặc chạy trong dây cáp (Wired) để đọc trộm dữ liệu, mật khẩu.
*   **Tại sao quan trọng:** Đây là cách cực kỳ im lặng và thụ động để lấy thông tin nhạy cảm. Nếu hệ thống mạng không áp dụng mã hóa (Encryption) hoặc cấu hình Switch yếu kém, Hacker chỉ cần cắm máy tính vào mạng LAN là có thể thu hoạch toàn bộ User, Passwords, Emails truyền qua mạng.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Sniffing (Nghe lén):** Quá trình chụp (Capture) và giám sát dữ liệu truyền qua một mạng máy tính.
*   **Passive Sniffing (Thụ động):** Chỉ hoạt động trên các Hub cũ. Dữ liệu broadcast đến mọi cổng, Hacker chỉ cần bật chế độ Promiscuous Mode là bắt được gói tin. Rất khó phát hiện.
*   **Active Sniffing (Chủ động):** Áp dụng trên các Switch hiện đại (chỉ gửi dữ liệu đến đúng cổng đích). Hacker phải dùng các kỹ thuật "chủ động" (như ARP Spoofing, MAC Flooding) để ép Switch nhả dữ liệu ra toàn mạng. Dễ bị phát hiện.
*   **Promiscuous Mode:** Chế độ đặc biệt của Card mạng (NIC). Bình thường NIC chỉ nhận gói tin gửi cho MAC của nó. Khi bật Promiscuous, NIC sẽ gom TẤT CẢ gói tin nó thấy trên đường truyền.
*   **CAM Table (Content Addressable Memory):** Bảng ánh xạ giữa địa chỉ MAC và Cổng (Port) vật lý nằm bên trong bộ nhớ của Switch.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **ARP Spoofing / Poisoning** | Kỹ thuật đầu độc bảng ARP để đánh lừa máy nạn nhân rằng Hacker là Router. |
| 🔴 **MUST MEMORIZE** | **MAC Flooding** | Bắn hàng ngàn địa chỉ MAC giả vào Switch làm đầy bảng CAM, biến Switch thành Hub. |
| 🔴 **MUST MEMORIZE** | **Promiscuous Mode** | Cài đặt phần cứng bắt buộc trên Card mạng để nghe lén gói tin không thuộc về mình. |
| 🟠 **HIGH PRIORITY** | **DHCP Starvation** | Tấn công vét cạn toàn bộ địa chỉ IP của DHCP Server. |
| 🟠 **HIGH PRIORITY** | **Rogue DHCP Server** | Hacker dựng máy chủ DHCP giả mạo để cấp IP và trỏ DNS/Gateway về máy Hacker. |
| 🟡 **SHOULD KNOW** | **SPAN Port (Port Mirroring)** | Tính năng của Switch hợp lệ dùng để copy gói tin sang một cổng khác cho IDS/Sniffer. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Wireshark** | Packet Sniffer / Analyzer | Phân tích gói tin mạng chuyên sâu nhất thế giới (GUI). Dùng để đọc file `.pcap`. | Tool "quốc dân", bắt buộc phải biết bộ lọc (Filter). |
| **Tcpdump** | Packet Sniffer | Công cụ bắt gói tin giao diện dòng lệnh (CLI) cực nhanh, thường có sẵn trên Linux. | Chạy nền rất tốt trên server. |
| **Cain & Abel** | Sniffer / Cracker | Công cụ cổ điển trên Windows chuyên làm ARP Poisoning, trích xuất mật khẩu. | Thường gặp trong đáp án nhiễu hoặc hỏi Tool Windows. |
| **Ettercap / Bettercap** | MiTM Tool | Framework chuyên dùng cho các cuộc tấn công Man-in-the-Middle, DNS/ARP Spoofing. | Cực mạnh, thường dùng trên Kali Linux. |
| **Macof** | MAC Flooding Tool | Gửi ngập lụt hàng vạn địa chỉ MAC giả vào mạng để tràn bảng CAM của Switch. | Lệnh quen thuộc: `macof -i eth0`. |
| **Zeek (Bro) + AI** | **AI-Powered IDS/Sniffer** | Phân tích lưu lượng mạng bằng AI để tìm ra các hành vi nghe lén hoặc C&C botnet. | Blue Team dùng AI để chống Sniffing. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> CEH exam yêu cầu biết cơ bản về cú pháp lọc (Filter) của Wireshark và Tcpdump.

*   **Wireshark Display Filters:**
    *   `ip.addr == 192.168.1.1`: Lọc tất cả các gói tin đến hoặc đi từ IP này.
    *   `tcp.port == 80`: Lọc tất cả gói tin HTTP.
    *   `http.request.method == "POST"`: Lọc gói tin POST (thường dùng để tìm username/password được submit).
*   **Tcpdump Commands:**
    *   `tcpdump -i eth0`: Lắng nghe trên cổng mạng eth0.
    *   `tcpdump -w capture.pcap`: Lưu kết quả bắt được vào file `.pcap`.
    *   `tcpdump -nn`: Không phân giải IP thành tên miền, hiển thị số Port (chạy nhanh hơn).

## 6. ATTACKS & TECHNIQUES
*   **MAC Flooding:** Switch dùng bảng CAM (bộ nhớ có hạn) để nhớ Port nào cắm thiết bị nào (MAC). Hacker dùng `macof` bắn hàng ngàn MAC giả mạo vào mạng. Bảng CAM bị đầy -> Switch "hoảng loạn" (Fail Open) và bắt đầu phát tán dữ liệu ra TẤT CẢ các cổng giống như mạng Hub cũ -> Hacker nghe lén được mọi thứ.
*   **ARP Spoofing (ARP Poisoning):** Kẻ tấn công gửi thông điệp ARP giả mạo vào mạng LAN, tuyên bố: *"Địa chỉ MAC của tôi là địa chỉ của Router (Default Gateway)"*. Máy nạn nhân tin lời, cập nhật bảng ARP tĩnh. Kết quả: Mọi gói tin nạn nhân muốn ra Internet đều phải đi qua máy Hacker trước (Man-in-the-Middle).
*   **DHCP Starvation:** Dùng tool Yersinia hoặc DHCPig gửi hàng ngàn yêu cầu xin cấp IP với MAC giả, rút cạn "bể bơi" IP của mạng nội bộ. Khiến user mới vào mạng không có IP.
*   **Rogue DHCP Server:** Sau khi rút cạn IP (hoặc không cần), Hacker dựng lên 1 máy chủ DHCP giả mạo cấp phát IP nhanh hơn máy chủ thật. Lợi ích: Hacker ép nạn nhân dùng Default Gateway và DNS Server do Hacker chỉ định.
*   **DNS Spoofing:** Hacker đánh lừa máy nạn nhân, khi nạn nhân gõ `facebook.com`, máy tự động trỏ về IP trang lừa đảo của Hacker.

## 7. PROTOCOLS/PORTS/SERVICES
Những giao thức **truyền văn bản gốc (Clear-text)** là "miếng mồi ngon" cho Sniffing. Khi thi CEH, nếu câu hỏi bảo hãy chọn giao thức để bảo mật, LUÔN chọn giao thức có chữ "S" (Secure/SSH) ở đuôi.
*   **Dễ bị đọc trộm:** Telnet (Port 23), FTP (21), HTTP (80), POP3 (110), IMAP (143), SNMP v1/v2 (161).
*   **Thay thế an toàn:** SSH (Port 22 thay Telnet), SFTP/FTPS (thay FTP), HTTPS (443 thay HTTP), POP3S (995), IMAPS (993), SNMP v3.

## 8. IMPORTANT NUMBERS & FACTS
*   **Bảng CAM (CAM Table):** Hoạt động ở Layer 2 của mô hình OSI (Data Link Layer).
*   Bảo vệ khỏi MAC Flooding: Kích hoạt tính năng **Port Security** trên Switch (ví dụ: giới hạn 1 cổng vật lý chỉ được ghi nhận 2 địa chỉ MAC tĩnh).
*   Bảo vệ khỏi ARP Spoofing: Kích hoạt tính năng **Dynamic ARP Inspection (DAI)** trên thiết bị mạng Cisco.

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Active Sniffing | Passive Sniffing |
| :--- | :--- | :--- |
| **Môi trường hoạt động** | Mạng **Switch** (Hệ thống mạng hiện đại). | Mạng **Hub** (Rất cũ, hiếm gặp). |
| **Bản chất** | Chủ động tung kỹ thuật ép Switch nhả dữ liệu. | Im lặng ngồi hứng gói tin (Promiscuous Mode). |
| **Kỹ thuật** | ARP Spoofing, MAC Flooding, DNS Spoofing. | Bật Wireshark ngồi nhìn. |
| **Khả năng bị phát hiện** | Dễ bị phát hiện (Cảnh báo lượng ARP, MAC lớn). | Cực khó phát hiện (Không tương tác). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Nguyên nhân Switch thành Hub:** Đề bài: *"Nhân viên an ninh mạng thấy Switch bỗng nhiên phát tán lưu lượng ra toàn bộ các cổng giống như Hub. Cuộc tấn công nào đang diễn ra?"* -> Đáp án: **MAC Flooding**. (Do tràn bảng CAM - CAM Table Overflow).
*   **Man in the Middle (MiTM):** Đề hỏi: *"Kỹ thuật nào lừa máy tính nội bộ gửi lưu lượng đến máy của kẻ tấn công bằng cách giả mạo địa chỉ của Gateway?"* -> Đáp án: **ARP Poisoning / ARP Spoofing**.
*   **Biện pháp phòng thủ:** 
    *   Hỏi cách chống MAC Flooding -> Chọn **Port Security**.
    *   Hỏi cách chống ARP Spoofing -> Chọn **Dynamic ARP Inspection (DAI)**.
    *   Hỏi cách chống DHCP Starvation -> Chọn **DHCP Snooping**.
*   **Bắt Password cleartext:** Đề hỏi: *"Giao thức nào dưới đây dễ bị lộ mật khẩu khi bị Sniffing nhất?"* -> Tìm các đáp án Telnet, FTP, HTTP.

## 11. COMMON CONFUSIONS
*   **DHCP Starvation vs Rogue DHCP:** DHCP Starvation là hành động "rút cạn" IP hợp lệ (mang tính phá hoại - DoS). Trái lại, Rogue DHCP Server là hành động lừa đảo, dựng DHCP giả mạo để kiểm soát hệ thống mạng của nạn nhân (MiTM). Thường Starvation là bước đệm để tung ra Rogue DHCP.
*   **MAC Spoofing vs MAC Flooding:** Spoofing là làm giả MAC của bạn thành MAC hợp lệ của ai đó (để vượt qua bộ lọc MAC filter). Flooding là xả rác hàng vạn MAC ảo ngẫu nhiên để đánh sập bộ nhớ Switch.

## 12. REAL-WORLD CONTEXT
*   **Coffee Shop Wi-Fi:** Mạng Wi-Fi công cộng (không có mật khẩu WPA2 Enterprise) là thiên đường của Sniffing. Hacker mở Wireshark ở quán cà phê là có thể bắt được cookies, hình ảnh không mã hóa, mật khẩu FTP của những người ngồi xung quanh. Do đó, DÙNG VPN MỌI LÚC là lời khuyên bắt buộc trên mạng công cộng.
*   **Port Mirroring (SPAN):** Trong thực tế doanh nghiệp, người quản trị mạng hợp pháp cũng dùng Sniffing. Họ cấu hình một cổng Switch là cổng SPAN (Mirror port), nó sẽ nhân bản (copy) tất cả lưu lượng của mạng chuyển vào cổng SPAN đó, nơi đang gắn hệ thống Cảnh báo xâm nhập (IDS) để phân tích bằng AI.

## 13. QUICK REVISION
1.  **Chế độ nào phải được bật trên Card mạng (NIC) để nó có thể nghe lén mọi gói tin?** -> Promiscuous Mode.
2.  **Kỹ thuật gì gây tràn bảng nhớ (CAM Table) của Switch?** -> MAC Flooding.
3.  **Công cụ `macof` dùng để làm gì?** -> Tấn công MAC Flooding.
4.  **Hành động gửi hàng loạt truy vấn lấy IP giả để làm cạn kiệt Pool IP của mạng là gì?** -> DHCP Starvation.
5.  **Biện pháp tốt nhất chống lại MAC Flooding?** -> Switch Port Security.
6.  **Giao thức nào thay thế an toàn (mã hóa) cho Telnet?** -> SSH (Port 22).

## 14. MEMORY HOOKS
*   **ARP Spoofing:** Tưởng tượng bạn dán đè bảng tên của phòng Giám đốc lên phòng của bạn. Mọi nhân viên (máy tính) có tài liệu mật đều mang nhầm vào nộp cho bạn (Hacker).
*   **MAC Flooding:** Gửi thư rác ồ ạt làm người đưa thư lú lẫn, kết quả là ổng vứt thư ra cho mọi người tự nhặt. (Fail Open).
*   **Wireshark = Cá Mập Bắt Gói Tin.**
