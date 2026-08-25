# 📚 CEH v13 Study Notes - Module 12: Evading IDS, Firewalls, and Honeypots

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Các thế hệ Firewall và IDS hiện tại đã được nâng cấp bằng AI (AI-based WAF/NGFW) để nhận diện hành vi bất thường thay vì Signature cũ. Kẻ tấn công cũng dùng AI để sinh Payload ngẫu nhiên lách luật. Cuộc chiến nay là AI đấu với AI. Nhưng nếu bạn không hiểu cấu trúc gói tin (Fragmentation, TTL), AI không giúp bạn lách được tường lửa vật lý."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân loại 3 hệ thống phòng thủ: Firewall (Tường lửa), IDS/IPS (Cảnh báo/Ngăn chặn xâm nhập) và Honeypot (Hũ mật). Đây là bộ ba lá chắn cốt lõi."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Cách hệ thống phòng thủ mạng (Firewall, IDS, Honeypot) hoạt động và các kỹ thuật tinh vi để vượt qua (Evade) chúng. Bao gồm việc cắt nhỏ gói tin, giả mạo IP, và giấu địa chỉ nguồn.
*   **Tại sao quan trọng:** Đây là kiến thức sống còn của Hacker và Pentester. Nếu không biết cách vượt rào, mọi kỹ thuật quét mạng (Scanning) hay khai thác (Exploitation) đều sẽ bị Firewall chặn lại và IDS sẽ hú còi báo động cho quản trị viên khóa IP của bạn ngay tức khắc.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Firewall (Tường lửa):** Lính gác cổng. Ngăn chặn hoặc cho phép lưu lượng mạng dựa trên các bộ quy tắc (Rules) đã định sẵn (ví dụ: Chặn Port 445, Cho phép Port 80).
*   **IDS (Intrusion Detection System):** Hệ thống cảnh báo xâm nhập. Như một chiếc camera an ninh, nó chỉ *nhìn và báo động* khi có kẻ gian, nhưng không thể chặn kẻ gian lại.
*   **IPS (Intrusion Prevention System):** Hệ thống ngăn chặn xâm nhập. Như một anh bảo vệ có vũ trang, vừa báo động vừa lao ra *ngăn chặn* kết nối độc hại.
*   **Honeypot (Hũ mật):** Hệ thống mồi nhử. Một máy chủ giả lập cố tình để hớ hênh lỗ hổng nhằm dụ Hacker tấn công vào. Mục đích là để phân tích hành vi của Hacker và đánh lạc hướng khỏi hệ thống thật.
*   **Evasion (Lẩn tránh):** Nghệ thuật định dạng lại các gói tin tấn công sao cho chúng trông có vẻ hợp lệ khi đi qua Firewall/IDS, nhưng khi đến đích lại ráp nối lại thành mã độc.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Packet Fragmentation** | (Cắt nhỏ gói tin). Kỹ thuật kinh điển nhất để qua mặt IDS bằng cách chia nhỏ payload (Nmap `-f`). |
| 🔴 **MUST MEMORIZE** | **Source Routing** | Hacker tự chỉ định đường đi (những trạm router) cho gói tin thay vì để mạng tự quyết định (nhằm lách qua IDS). |
| 🔴 **MUST MEMORIZE** | **Honeynet** | Một mạng lưới gồm nhiều Honeypot liên kết với nhau để dụ Hacker (Thường nằm ở vùng DMZ). |
| 🟠 **HIGH PRIORITY** | **Obfuscation** | Làm rối mã (Mã hóa Hex/Base64/URL Encoding) để qua mặt bộ lọc chữ ký (Signature) của IDS/WAF. |
| 🟠 **HIGH PRIORITY** | **Session Splicing** | Tách nhỏ dữ liệu Session thành nhiều đoạn để IDS không kịp ghép nối và phân tích khối dữ liệu hoàn chỉnh. |
| 🟡 **SHOULD KNOW** | **Stateful Inspection** | Thế hệ Firewall kiểm tra toàn bộ trạng thái kết nối (State) thay vì chỉ nhìn từng gói tin rời rạc. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Snort** | IDS/IPS | Công cụ IDS nguồn mở huyền thoại. Dựa trên luật (Rule-based). | Rất hay có trong đề thi (đọc Rule Snort). |
| **KFSensor / Honeybot** | Honeypot | Các phần mềm giả lập hũ mật trên Windows để dụ Hacker tấn công. | Dùng để phân tích hành vi hacker. |
| **Nmap** | Evasion Tool | Nmap có cực nhiều cờ lẩn tránh: `-f` (phân mảnh), `-D` (decoy), `-S` (spoof IP). | Bắt buộc thuộc lòng các cờ này. |
| **ProxyChains / Tor** | Anonymizer | Định tuyến gói tin qua nhiều proxy trung gian để giấu IP thật khỏi hệ thống phòng thủ. | Cốt lõi của việc ẩn danh (Anonymity). |
| **Palo Alto / Fortinet** | NGFW | Next-Generation Firewall. Tích hợp AI để phát hiện tấn công sâu (DPI). | Đại diện cho hệ thống phòng thủ hiện đại. |
| **AI Payload Mutators** | **AI-Powered** | Kẻ tấn công dùng AI để tự động sinh ra các chuỗi mã hóa (Obfuscation) thay đổi liên tục nhằm vượt qua WAF (Web App Firewall). | Kỹ thuật lách luật hiện đại v13. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> CEH exam yêu cầu biết kỹ các câu lệnh Nmap dùng để vượt rào Firewall.

*   `nmap -f <IP>`: **Fragmentation**. Cắt nhỏ phần Header TCP thành nhiều mảnh 8-byte để IDS không đọc được toàn bộ.
*   `nmap -mtu 24 <IP>`: **Tùy chỉnh MTU**. Phân mảnh theo kích thước tự chọn (ở đây là 24 byte).
*   `nmap -D decoy1,decoy2,ME <IP>`: **Decoy Scan**. Chèn địa chỉ IP giả (Decoy) vào chung luồng quét với IP thật (ME). IDS sẽ báo động mù mịt hàng vạn IP, làm nhiễu hệ thống.
*   `nmap --source-port 53 <IP>`: **Giả mạo cổng nguồn**. Firewall thường cho phép cổng DNS (53) đi qua. Hacker giả vờ gói tin của mình xuất phát từ cổng 53 để lừa Firewall.

## 6. ATTACKS & TECHNIQUES
### Các Kỹ Thuật Lách IDS/Firewall (Evasion Techniques):
1.  **Insertion (Chèn):** Hacker cố tình nhét thêm "rác" vào gói tin (IDS không đọc được nhưng mục tiêu lại bỏ rác đi và chạy mã độc).
2.  **Evasion (Lẩn tránh):** Gửi gói tin chia nhỏ, IDS ráp không kịp và bỏ qua. Nhưng HĐH máy đích lại ráp được và bị nhiễm độc.
3.  **Obfuscation (Làm rối):** IDS tìm chuỗi `SELECT * FROM users`. Hacker mã hóa nó thành `SELECT%20%2A%20FROM%20users` (URL Encoding). IDS không nhận ra nhưng Web Server vẫn hiểu và bị SQLi.
4.  **Session Splicing (Ghép phiên):** Tương tự phân mảnh, nhưng ở mức ứng dụng. Hacker giữ session chạy rất chậm, chia chuỗi tấn công làm 10 phần, gửi mỗi phần cách nhau vài giây khiến IDS mất kiên nhẫn và xả bộ nhớ.
5.  **Firewall Walking:** Kỹ thuật dùng độ lệch TTL (Time To Live) để dò xem đằng sau Firewall có máy nào không (thường dùng công cụ Firewalk).

## 7. PROTOCOLS/PORTS/SERVICES
*   **Honeypot Ports:** Honeypot thường mở toang các cổng hấp dẫn (21 FTP, 22 SSH, 23 Telnet, 445 SMB, 3389 RDP) để thu hút sự chú ý.
*   **ICMP (Type 3 Code 13):** "Communication Administratively Prohibited". Đây là thông điệp chuẩn Firewall trả về khi nó chặn một gói tin của Hacker. (Ghi chú: Nếu Firewall để chế độ DROP/Stealth, nó sẽ không trả về gì cả).

## 8. IMPORTANT NUMBERS & FACTS
*   **Phân loại Firewall:**
    *   **Packet Filtering (Thế hệ 1):** Chỉ xét địa chỉ IP, Port nguồn/đích. Hoạt động ở Lớp 3 (Network). Rất nhanh nhưng ngu ngốc.
    *   **Circuit-Level Gateway (Thế hệ 2):** Kiểm tra quá trình bắt tay TCP Handshake. Hoạt động ở Lớp 5 (Session).
    *   **Stateful Inspection (Thế hệ 3):** Lưu giữ trạng thái của toàn bộ kết nối. Lớp 3 + Lớp 4.
    *   **Application Level (Proxy/WAF):** Kiểm tra sâu vào tận nội dung dữ liệu (Lớp 7 - Application Layer). (Ví dụ: Chặn tải file `.exe`).

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | IDS (Intrusion Detection) | IPS (Intrusion Prevention) |
| :--- | :--- | :--- |
| **Hành động** | Chỉ Cảnh Báo (Alert/Log). | Cảnh báo và **Ngăn chặn** (Block/Drop). |
| **Vị trí** | Thường đặt Out-of-band (Nghe lén từ SPAN port). | Phải đặt In-line (Gói tin bắt buộc phải đi qua nó). |
| **Rủi ro** | Có cảnh báo giả (False Positive) nhưng không làm sập mạng. | Báo động giả có thể khóa IP của khách hàng thật (Rất nguy hiểm). |

| Tiêu chí | Low-interaction Honeypot | High-interaction Honeypot |
| :--- | :--- | :--- |
| **Độ chân thực** | Thấp (Chỉ mở port mô phỏng dịch vụ). | Rất cao (Một máy chủ thật có lỗ hổng thật). |
| **Rủi ro** | Ít rủi ro. | Rủi ro cao (Hacker có thể chiếm hũ mật làm bàn đạp tấn công tiếp). |
| **Dữ liệu thu được** | Ít (Chỉ biết IP, Port bị scan). | Nhiều (Biết được toàn bộ lệnh, mã độc Hacker tải lên). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Source Routing:** Câu hỏi: *"Hacker tự định nghĩa đường dẫn (route) cho gói tin TCP đi qua các node mạng tự chọn thay vì qua Firewall. Đây là kỹ thuật gì?"* -> Chọn **Source Routing**.
*   **Đọc Rule Snort:** Đề cho 1 dòng `alert tcp $EXTERNAL_NET any -> $HTTP_SERVERS 80 (msg:"SQL Injection Attempt"; content:"SELECT";)`
    *   *Câu hỏi:* Rule này làm gì? -> *Đáp án:* Cảnh báo (alert) bất kỳ luồng TCP nào đi từ ngoài vào máy chủ HTTP cổng 80 mà có chứa chữ "SELECT".
*   **Cờ Nmap Evasion:** Hỏi: *"Kỹ thuật lẩn tránh nào làm xáo trộn log của IDS bằng cách đưa hàng ngàn IP giả xen kẽ với IP thật?"* -> Chọn **Decoy Scan (-D)**.
*   **Phát hiện Honeypot:** Hỏi: *"Cách tốt nhất để Hacker phát hiện 1 hệ thống là Honeypot?"* -> Khai thác các dịch vụ, nếu thấy nó phản hồi rất "chuẩn mực" nhưng môi trường bên trong hoàn toàn rỗng hoặc hành vi máy bất thường (dịch vụ phản hồi quá nhanh, không có dữ liệu thật).
*   **Firewall Bypass:** Đề hỏi: *"Gửi truy vấn HTTP nhưng ngụy trang trên cổng 53 (DNS) để qua mặt tường lửa lọc gói tin (Packet Filtering). Gọi là gì?"* -> Chọn **Port Evasion / Source Port Spoofing**.

## 11. COMMON CONFUSIONS
*   **Firewall vs IPS:** Firewall chủ yếu đóng/mở cửa dựa trên *địa chỉ* (Cho vào / Cấm vào). IPS kiểm tra *hành vi* (Mày vào được rồi nhưng mày ăn trộm là tao bắt).
*   **Fragmentation vs Session Splicing:** Fragmentation (Phân mảnh) xảy ra ở tầng mạng (Layer 3 - TCP/IP). Session Splicing (Ghép phiên) xảy ra ở tầng ứng dụng (Layer 7 - HTTP/App). Cả hai đều nhằm làm IDS không ghép nổi dữ liệu.

## 12. REAL-WORLD CONTEXT
*   **Web Application Firewall (WAF):** Trong thực tế, các cuộc tấn công SQLi / XSS đều bị WAF (như Cloudflare) chặn ngay lập tức. Hacker phải dùng các thủ thuật **Obfuscation** tinh vi (như chèn ký tự Null `%00`, dùng dấu nháy kép `""`, dùng chữ hoa chữ thường `sElEcT`) để AI của WAF không đọc được đoạn mã độc.
*   **Honeynet trong Enterprise:** Các ngân hàng lớn thường xây dựng 1 mạng lưới Honeynet hoàn chỉnh chứa các máy ATM ảo, Database ảo. Ngay khi có kẻ rà quét (Scan) chạm vào mạng ảo này, hệ thống sẽ phát chuông báo động cho SOC Team, vì nhân viên bình thường sẽ không bao giờ mò mẫm vào dải IP đó.

## 13. QUICK REVISION
1.  **Hệ thống nào cố tình tỏ ra yếu kém để nhử Hacker tấn công?** -> Honeypot.
2.  **Kỹ thuật Nmap nào cắt gói tin TCP thành các phần 8-byte để lẩn tránh IDS?** -> Fragmentation (`-f`).
3.  **Tường lửa thế hệ 3 lưu trữ toàn bộ trạng thái kết nối gọi là gì?** -> Stateful Inspection Firewall.
4.  **Kỹ thuật ngụy trang IP thật giữa một rừng IP giả khi quét mạng gọi là gì?** -> Decoy Scan.
5.  **Thuật ngữ chỉ việc mã hóa/biến đổi payload (ví dụ URL Encoding) để qua mặt bộ lọc Signature của IDS?** -> Obfuscation.
6.  **Sự khác biệt lớn nhất giữa IDS và IPS là gì?** -> IDS chỉ cảnh báo, IPS vừa cảnh báo vừa ngăn chặn (Drop gói tin).

## 14. MEMORY HOOKS
*   **Bộ 3 phòng thủ:** 
    *   **Firewall:** Lính gác cổng (Cấm vào/Cho vào).
    *   **IDS:** Chó canh nhà (Thấy trộm thì sủa, không biết cắn).
    *   **IPS:** Cảnh sát bảo vệ (Sủa + Cắn + Còng tay).
*   **Decoy (-D):** Decoy = "Bù nhìn". Rải một đống bù nhìn ra đồng để quạ (IDS) không biết đâu là người thật.
*   **Fragmentation:** Băm nhỏ vũ khí ra, tuồn qua cửa bảo vệ, vào trong lắp lại thành khẩu súng.
