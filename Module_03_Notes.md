# 📚 CEH v13 Study Notes - Module 03: Scanning Networks

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** CEH v13 đưa AI vào (tạo script tự động, tự phân tích kết quả Nmap), nhưng nếu bạn không hiểu **TCP 3-way Handshake** và các cờ TCP (SYN, ACK, RST), bạn sẽ không thể tùy biến hay hiểu AI đang làm gì."
> *   "Phương pháp học tốt nhất cho phần này là **tự vẽ tay sơ đồ mindmap** về Quy trình Scanning (6 bước) và vẽ sơ đồ 3-way handshake. Đề thi sẽ hỏi cặn kẽ từng bit, từng flag của gói tin."
> *   **Công thức gốc:** `Risk = Threat x Vulnerability` (Việc quét mạng chính là đi tìm Vulnerability để đánh giá Risk thực tế).

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Chuyển từ giai đoạn thu thập thông tin gián tiếp (Footprinting) sang giai đoạn **chủ động tương tác** với mạng mục tiêu (Active). Module này dạy cách phát hiện các máy đang "sống", cổng (port) nào đang mở, dịch vụ (service) nào đang chạy, hệ điều hành (OS) gì, và cấu trúc mạng ra sao.
*   **Tại sao quan trọng:** Quét mạng (Scanning) là bước chuẩn bị vũ khí trực tiếp. Dữ liệu từ bước này sẽ quyết định bạn dùng mã khai thác (Exploit) nào. Quét sai hoặc ồn ào sẽ làm hệ thống Cảnh báo xâm nhập (IDS/IPS) khóa IP của bạn ngay lập tức.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Host Discovery (Ping Sweep):** Kiểm tra xem trong một dải IP (vd: 192.168.1.0/24), những IP nào đang thực sự hoạt động.
*   **Port Scanning:** Gửi các gói tin đến mục tiêu để xem "cửa" (Port) nào đang mở, đóng, hoặc bị Firewall chặn (Filtered).
*   **Service Version Discovery:** Sau khi biết cổng mở (vd: Port 80), tìm hiểu xem phần mềm gì đang chạy trên đó, phiên bản bao nhiêu (vd: Apache 2.4.41).
*   **OS Discovery (Banner Grabbing):** Xác định hệ điều hành của mục tiêu (Windows, Linux, v.v.) dựa trên phản hồi của gói tin (TTL, Window Size).
*   **TCP 3-Way Handshake:** Giao thức kết nối chuẩn của TCP (SYN -> SYN/ACK -> ACK). Hiểu rõ cái này là chìa khóa để qua môn CEH.
*   **IDS/Firewall Evasion:** Các kỹ thuật "tàng hình" để quét mạng mà không bị tường lửa hay hệ thống phát hiện xâm nhập chặn lại.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **TCP Flags** | URG, ACK, PSH, RST, SYN, FIN (Nhớ câu thần chú: **U**nskilled **A**ttackers **P**ester **R**eal **S**ecurity **F**olks). |
| 🔴 **MUST MEMORIZE** | **SYN Scan (Stealth)** | Quét nửa mở (Half-open). Nhanh, phổ biến nhất, khó bị ghi log (vì không hoàn tất handshake). |
| 🔴 **MUST MEMORIZE** | **Inverse TCP Scans** | XMAS, FIN, NULL. Dựa trên nguyên lý: Gửi cờ lạ, nếu port đóng -> trả về RST. Nếu port mở -> im lặng. |
| 🔴 **MUST MEMORIZE** | **Nmap** | Vị vua của các công cụ quét mạng. Bắt buộc phải thuộc các tham số (flags) của Nmap. |
| 🟠 **HIGH PRIORITY** | **Packet Fragmentation** | Cắt nhỏ gói tin để đánh lừa IDS/Firewall (Nmap flag: `-f`). |
| 🟠 **HIGH PRIORITY** | **Decoy Scan** | Quét cùng với nhiều IP giả mạo để giấu IP thật của Hacker (Nmap flag: `-D`). |
| 🟡 **SHOULD KNOW** | **Hping3** | Công cụ chế tạo gói tin (Packet crafter) tùy chỉnh siêu mạnh. |
| 🟡 **SHOULD KNOW** | **RustScan / masscan** | Các công cụ quét siêu tốc (nhanh hơn Nmap nhiều lần) do dùng cơ chế bất đồng bộ. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)

> [!TIP]
> **[AI Overlay trong CEH v13]** Attackers có thể dùng AI (như ChatGPT, DorkGPT hoặc các công cụ tích hợp LLM) để tự động sinh các script Nmap (NSE - Nmap Scripting Engine) hoặc viết Python scanner tùy chỉnh để tự động hóa Host, Port, OS Discovery. AI giúp cấu trúc lại dữ liệu thô từ Nmap thành báo cáo dễ hiểu.

| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Nmap** | Network Scanner | Công cụ chuẩn công nghiệp để quét host, port, dịch vụ, OS. Dùng để lập sơ đồ mạng. | Bắt buộc phải thuộc cú pháp. |
| **Hping3** | Packet Crafter | Tạo ra các gói tin TCP, UDP, ICMP tùy chỉnh bằng command line. Dùng để test firewall, flood (DoS). | Không chỉ để scan, còn để tấn công DoS, ngụy tạo IP. |
| **Masscan** | High-speed Scanner | Quét toàn bộ Internet (cổng cụ thể) chỉ trong vài phút (cần băng thông lớn). | Nhanh hơn Nmap nhờ viết lại toàn bộ TCP/IP stack. |
| **RustScan** | High-speed Scanner | Quét siêu nhanh, sau đó tự động đẩy các cổng mở sang Nmap để quét sâu hơn. | Mới nổi, tối ưu hóa tốc độ. |
| **Angry IP Scanner** | Ping Sweep Tool | Công cụ GUI đơn giản, ping hàng loạt IP cực nhanh. | Rất hay được dùng để Host Discovery nội bộ. |
| **Zenmap** | GUI Scanner | Giao diện đồ họa chính thức của Nmap, hỗ trợ vẽ sơ đồ mạng (Network Topology). | Sinh sẵn các command Nmap. |
| **Metasploit** | Framework | Có các module auxiliary để quét mạng (vd: `auxiliary/scanner/portscan/tcp`). | Tích hợp sâu giữa Scan và Exploit. |
| **AI Assistants** | **AI-Powered** | Tự động sinh ra các đoạn mã quét tùy chỉnh, phân tích file XML của Nmap để tìm lỗ hổng logic. | AI không thay thế Nmap, AI *điều khiển* Nmap. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> **CEH EXAM TRỌNG TÂM:** Bạn PHẢI học thuộc bảng cú pháp Nmap này.

*   **Host Discovery (Ping Sweep):**
    *   `nmap -sn 192.168.1.0/24`: Quét xem host nào đang sống (Không quét port). (Trước đây là `-sP`).
*   **Port Scanning (Các kiểu quét cổng):**
    *   `nmap -sT <IP>`: **TCP Connect Scan**. Quét đầy đủ (Hoàn tất 3-way handshake). Rất dễ bị phát hiện và ghi log.
    *   `nmap -sS <IP>`: **SYN / Stealth Scan**. Gửi SYN, nhận SYN/ACK, trả RST. Cực kỳ phổ biến, yêu cầu quyền Root/Admin.
    *   `nmap -sU <IP>`: **UDP Scan**. Chậm, dùng để tìm các cổng như DNS (53), SNMP (161).
    *   `nmap -sX <IP>`: **XMAS Scan**. Bật các cờ FIN, URG, PSH. (Sáng như cây thông Noel).
    *   `nmap -sN <IP>`: **NULL Scan**. Không bật bất kỳ cờ TCP nào.
    *   `nmap -sA <IP>`: **ACK Scan**. Chỉ bật cờ ACK, dùng để **kiểm tra Firewall** (xem port là Filtered hay Unfiltered), không dùng để xem port mở/đóng.
*   **Version & OS Discovery:**
    *   `nmap -O <IP>`: **OS Fingerprinting**. Đoán hệ điều hành (dùng TCP/IP stack fingerprinting).
    *   `nmap -sV <IP>`: **Service Version**. Đoán phiên bản dịch vụ đang chạy trên cổng mở.
    *   `nmap -A <IP>`: **Aggressive Scan**. "Combo 4 trong 1" (OS, Version, Traceroute, Script scanning). Cực kỳ ồn ào.
*   **IDS/Firewall Evasion:**
    *   `nmap -f <IP>`: Fragment (Cắt nhỏ gói tin).
    *   `nmap -D decoy1,decoy2,ME <IP>`: Dùng địa chỉ IP Decoy để giấu IP thật.

## 6. ATTACKS & TECHNIQUES

*   **TCP 3-Way Handshake (Nguyên lý):**
    1. Client gửi **SYN** (Chào, cho tôi kết nối).
    2. Server gửi **SYN/ACK** (Chào, tôi đồng ý kết nối).
    3. Client gửi **ACK** (Được, bắt đầu nói chuyện).
*   **Banner Grabbing:** Kỹ thuật lấy thông tin (OS, Version) của dịch vụ.
    *   **Active:** Gửi gói tin sai chuẩn, xem cách mục tiêu trả lời (Nmap `-O`, Telnet, Netcat).
    *   **Passive:** Bắt gói tin (Sniffing bằng Wireshark) và phân tích TTL, Window Size.
*   **IP Spoofing (Giả mạo IP):** Thay đổi địa chỉ IP nguồn trong gói tin để hệ thống đích tưởng gói tin đến từ một nguồn đáng tin cậy. (Lưu ý: Bạn không thể nhận được phản hồi nếu dùng IP ảo, trừ khi dùng Source Routing).
*   **Vulnerability Scanning:** Giai đoạn sau của quét mạng (thường dùng Nessus, OpenVAS) để so sánh các dịch vụ/OS tìm được với cơ sở dữ liệu lỗ hổng (CVE). Nmap có hỗ trợ qua Nmap Scripting Engine (NSE) - `nmap --script vuln`.

## 7. PROTOCOLS/PORTS/SERVICES
| Protocol | TCP Flags (Cực quan trọng) | Nguyên lý phản hồi mặc định (RFC 793) |
| :--- | :--- | :--- |
| **TCP** | **SYN** (Synchronize) | Khởi tạo kết nối |
| **TCP** | **ACK** (Acknowledgment) | Xác nhận đã nhận gói tin |
| **TCP** | **RST** (Reset) | Đóng kết nối ngay lập tức / Báo lỗi (Port đóng thường trả về RST) |
| **TCP** | **FIN** (Finish) | Kết thúc kết nối bình thường |
| **TCP** | **PSH** (Push) | Đẩy dữ liệu lên application layer ngay lập tức |
| **TCP** | **URG** (Urgent) | Dữ liệu khẩn cấp |
| **ICMP** | **Type 8 / Type 0** | Echo Request (Ping đi) / Echo Reply (Ping về) |

## 8. IMPORTANT NUMBERS & FACTS
*   **65,535:** Tổng số lượng cổng (Ports) TCP và UDP có thể có.
*   **1000:** Số lượng cổng mặc định mà Nmap sẽ quét (các cổng phổ biến nhất) nếu không chỉ định cờ `-p`.
*   Để quét toàn bộ cổng: Dùng tham số `-p-` (quét từ cổng 1 đến 65535).
*   **TTL Windows mặc định:** Thường là **128**.
*   **TTL Linux mặc định:** Thường là **64**. (Passive Banner Grabbing dùng số này để đoán OS).

## 9. COMPARE & DIFFERENTIATE

### ⚖️ TCP Connect Scan (-sT) vs SYN Stealth Scan (-sS)
| Tiêu chí | TCP Connect Scan (-sT) | SYN Stealth Scan (-sS) |
| :--- | :--- | :--- |
| **Gói tin gửi đi** | SYN -> nhận SYN/ACK -> gửi **ACK** | SYN -> nhận SYN/ACK -> gửi **RST** |
| **Hoàn tất Handshake?** | Có (100% hoàn thành) | Không (Nửa chừng / Half-open) |
| **Khả năng bị Ghi Log** | Rất dễ (Ứng dụng đã ghi log kết nối) | Khó bị ghi log (Vì ứng dụng chưa kịp mở session) |
| **Quyền thực thi** | Người dùng bình thường | **Cần quyền Root/Administrator** |

### ⚖️ XMAS, FIN, NULL (Inverse Scans)
> [!WARNING]
> Quy luật bất hủ: Nếu cổng **ĐÓNG**, nó sẽ chửi lại (trả về **RST**). Nếu cổng **MỞ**, nó sẽ **IM LẶNG** (No response).
> *Lưu ý:* Các scan này thường không hoạt động trên hệ điều hành Windows hiện đại (Windows trả về RST bất kể cổng mở hay đóng).

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Dịch kết quả Nmap:** Đề thi sẽ đưa ra một dòng lệnh Nmap và hỏi mục đích. Hoặc đưa ra kết quả và hỏi bạn đã dùng cờ gì.
    *   Ví dụ: *"Hacker muốn quét mà không để lại log trên máy chủ Windows 2003, lệnh nào tốt nhất?"* -> Chọn `-sS` (SYN Scan).
*   **Nhận diện loại Scan qua Wireshark:** Đề thi đưa ảnh chụp Wireshark (Client gửi FIN, URG, PSH). Hỏi đây là kiểu scan gì? -> Chọn **XMAS Scan**.
*   **ACK Scan:** Nếu câu hỏi đề cập đến *"mapping firewall rules"*, *"determining if port is filtered or unfiltered"*, chọn **ACK Scan (`-sA`)**. Nó KHÔNG dùng để xem cổng đóng hay mở!
*   **UDP Scan:** Quét cổng 53, 161, 123 -> Chọn `-sU`.
*   **Phân biệt Hping3:** Nếu câu hỏi cần tạo gói tin (Packet crafting), test firewall rules thủ công -> Chọn **Hping3**.

## 11. COMMON CONFUSIONS
*   **Host Discovery vs Port Scanning:** Đừng gộp chung. Host Discovery (Ping Sweep) chỉ để xem máy tính có bật không (IP có sống không). Port Scanning là xem trên cái máy đã bật đó, có cửa nào (Port) đang mở. Nmap `-sn` làm bước 1, bỏ qua bước 2.
*   **Active vs Passive Banner Grabbing:** Active gửi gói tin dị thường đến máy đích. Passive thì ngồi yên cắm cáp (sniff) hoặc vớt gói tin trên mạng để đọc TTL mà không gửi gì cho mục tiêu cả.

## 12. REAL-WORLD CONTEXT
*   Trong thực tế, **Nmap rất ồn ào**. Nếu bạn quét toàn bộ 65535 cổng bằng `-sT` vào mạng doanh nghiệp, đội SOC (Blue Team) sẽ phát hiện ra bạn trong vòng chưa tới 1 phút.
*   Pentester thực tế thường dùng `masscan` hoặc `RustScan` để quét toàn bộ dải IP rộng (tìm các port mở nhanh nhất), sau đó đưa danh sách port mở ngược lại cho `nmap -sV -sC` để quét chi tiết dịch vụ. Tiết kiệm hàng giờ đồng hồ.
*   **Decoy Scan (-D)** rất hữu ích. Thay vì chỉ mình bạn IP 1.1.1.1 đi quét, bạn gắn Decoy 8.8.8.8, 1.2.3.4. Hệ thống IDS sẽ thấy hàng loạt IP đang quét nó và không biết ai là kẻ tấn công thực sự.

## 13. QUICK REVISION
1.  **Cờ Nmap nào dùng để thực hiện quét nửa mở (Stealth)?** -> `-sS`
2.  **Quét XMAS bật những cờ TCP nào?** -> URG, PSH, FIN.
3.  **Hệ điều hành nào phản hồi lại XMAS scan bằng RST bất kể cổng đóng hay mở?** -> Windows.
4.  **Cờ Nmap nào dùng để đoán hệ điều hành (OS Discovery)?** -> `-O`
5.  **Kỹ thuật Nmap nào cắt nhỏ gói tin để vượt Firewall?** -> Fragmentation (`-f`).
6.  **Quá trình TCP 3-way handshake diễn ra như thế nào?** -> SYN -> SYN/ACK -> ACK.

## 14. MEMORY HOOKS
*   **Cờ TCP:** **U**nskilled **A**ttackers **P**ester **R**eal **S**ecurity **F**olks (URG, ACK, PSH, RST, SYN, FIN).
*   **Nmap Flags:** Chữ cái viết HOA là loại quét (Scan Type), chữ cái nhỏ đứng đầu báo kiểu.
    *   `-s` = Scan.
    *   `-sS` = Scan **S**YN (Stealth).
    *   `-sT` = Scan **T**CP (Connect).
    *   `-sU` = Scan **U**DP.
    *   `-sX` = Scan **X**mas.
    *   `-O` = **O**perating System.
    *   `-sV` = Scan **V**ersion.
    *   `-A` = **A**ggressive (Tất cả trong 1 - All in one).
