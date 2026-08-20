# 📚 CEH v13 Study Notes - Module 02: Footprinting and Reconnaissance

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Dù CEH v13 giới thiệu các công cụ AI OSINT cực mạnh, nhưng kiến thức mạng cơ bản (TCP/IP, DNS, OSI) mới là cốt lõi để bạn thực sự hiểu mục tiêu."
> *   "Đối với các bước và phân loại trong Footprinting, **hãy tự vẽ tay sơ đồ Mindmap**. Việc vẽ tay sẽ giúp não bộ ghi nhớ sâu hơn 10 lần so với việc chỉ nhìn vào màn hình máy tính. Không cần đẹp, chỉ cần hiệu quả!"
> *   **Công thức tư duy:** `Information Gathering + Analysis = Intelligence` (Thu thập thông tin + Phân tích = Tình báo).

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Module này hướng dẫn các phương pháp, kỹ thuật và công cụ để thu thập thông tin về mục tiêu (tổ chức, mạng lưới, cá nhân) **trước khi** tiến hành bất kỳ cuộc tấn công nào. Nó bao gồm từ việc thu thập qua các nguồn mở (OSINT) đến các tương tác nhẹ với hệ thống mục tiêu.
*   **Tại sao quan trọng:** Giai đoạn này chiếm tới **90% thời gian** của một Hacker chuyên nghiệp (và cả Ethical Hacker). Càng thu thập được nhiều thông tin, xác suất tấn công thành công càng cao, đồng thời thu hẹp phạm vi mục tiêu, phát hiện các điểm yếu tiềm năng để tiết kiệm thời gian ở các bước sau. Tôn tử đã nói: *"Biết người biết ta, trăm trận không nguy."*

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Footprinting (Dấu chân mạng):** Quá trình thu thập tối đa thông tin có thể về mạng lưới mục tiêu để vẽ nên một "bản đồ" (Network profile).
*   **Passive Footprinting (Thụ động):** Thu thập thông tin mà không tiếp xúc hoặc tương tác trực tiếp với máy chủ mục tiêu (thông qua bên thứ 3 như Google, Archive.org, Whois). Rất khó bị mục tiêu phát hiện.
*   **Active Footprinting (Chủ động):** Gửi gói tin hoặc tương tác trực tiếp với máy chủ mục tiêu để lấy thông tin (Ping, Traceroute, DNS Zone Transfer, gọi điện thoại giả danh). Nguy cơ bị ghi log (phát hiện) cao.
*   **OSINT (Open-Source Intelligence):** Tình báo nguồn mở. Quá trình thu thập, phân tích thông tin tình báo từ các nguồn công khai, hợp pháp (Website, MXH, báo chí, dark web).
*   **Competitive Intelligence:** Tình báo cạnh tranh. Hoạt động thu thập thông tin hợp pháp về đối thủ cạnh tranh từ các nguồn mở (báo cáo tài chính, tuyển dụng, báo chí) để phân tích chiến lược.
*   **Footprinting Objectives:**
    *   **Network Information:** Tên miền, Internal/External IP, Routing tables, DNS records.
    *   **System Information:** OS, Web servers, Passwords, Usernames.
    *   **Organization Information:** Employee details, Số điện thoại, Cơ cấu phòng ban, Vị trí địa lý.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Passive vs Active** | Phải phân biệt rõ ranh giới giữa việc thu thập qua bên thứ 3 và tương tác trực tiếp. |
| 🔴 **MUST MEMORIZE** | **Google Dorks** | Các toán tử tìm kiếm nâng cao để bới móc dữ liệu ẩn. |
| 🔴 **MUST MEMORIZE** | **DNS Zone Transfer** | (AXFR) Lỗ hổng nghiêm trọng làm lộ toàn bộ sơ đồ mạng thông qua DNS server. |
| 🔴 **MUST MEMORIZE** | **OSINT** | Tình báo nguồn mở - Nền tảng của Footprinting hiện đại. |
| 🟠 **HIGH PRIORITY** | **EDGAR** | Nguồn thu thập tài liệu tài chính công ty (Mỹ) cực tốt. |
| 🟠 **HIGH PRIORITY** | **Archive.org** | Wayback Machine - Cỗ máy thời gian xem web cũ. |
| 🟠 **HIGH PRIORITY** | **Shodan / Censys** | Search engine dành cho hacker (tìm IoT, Router, Server đang mở). |
| 🟡 **SHOULD KNOW** | **Netcraft** | Tool xem uptime, OS, Web Server của 1 domain. |
| 🟡 **SHOULD KNOW** | **FOCA** | Tool phân tích và trích xuất Metadata. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)

> [!TIP]
> **[AI Overlay]** CEH v13 bổ sung một loạt công cụ AI giúp tăng tốc OSINT. Hãy nhớ rằng các tool này dùng AI để thu thập và phân tích nhanh, nhưng dữ liệu vẫn xuất phát từ các kỹ thuật OSINT truyền thống.

| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Shodan / Censys** | IoT Search Engine | Tìm kiếm các thiết bị phần cứng kết nối Internet (camera, router, server, ICS/SCADA). | Google tìm **nội dung**, Shodan tìm **thiết bị**. |
| **Maltego** | OSINT Framework | Công cụ trực quan hóa (vẽ sơ đồ graph) mối quan hệ giữa các thực thể (người, domain, IP, cty). | Công cụ GUI cực mạnh để vẽ sơ đồ tình báo. |
| **Recon-ng** | OSINT Framework | Framework viết bằng Python chuyên thu thập thông tin mã nguồn mở. | Giao diện CLI tương tự Metasploit. |
| **FOCA** | Metadata Extractor | Fingerprinting Organizations with Collected Archives - Trích xuất metadata từ file PDF, DOCX để tìm user, OS, folder nội bộ. | Rất hay hỏi trong bài thi về trích xuất metadata. |
| **Netcraft** | Web Analysis | Trích xuất thông tin subdomains, thời gian uptime, hệ điều hành (OS), loại Web Server đang chạy. | Thụ động (Passive) thu thập thông tin web. |
| **Archive.org** | Web Archive | (Wayback Machine) Truy cập các phiên bản website trong quá khứ, dù đã bị xóa bỏ. | Giúp lấy lại các thông tin nhạy cảm đã bị ẩn đi. |
| **DNSdumpster / Sublist3r** | Subdomain Recon | Tìm kiếm subdomains và vẽ sơ đồ DNS của tổ chức. | Dùng trong DNS Footprinting. |
| **eMailTrackerPro** | Email Tracking | Phân tích Email Header để lần ra IP nguồn và theo dõi email. | Xác định nguồn gốc gửi email thật sự. |
| **Taranis AI / Cylect.io** | **AI-Powered OSINT** | Tự động hóa quá trình săn tìm thông tin tình báo, phân tích ngữ cảnh, tương quan dữ liệu lớn bằng AI. | Điểm mới trong v13. AI hỗ trợ OSINT. |
| **ChatPDF / DorkGPT** | **AI-Powered OSINT** | ChatPDF để trích xuất nhanh dữ liệu từ báo cáo. DorkGPT tự động tạo các câu lệnh Google Hacking phức tạp. | Dùng AI sinh Dorks thay vì gõ tay. |

*Các công cụ khác:* TheHarvester (thu thập email), Subfinder, Photon, OSINT Framework (web-based), Recon-Dog, BillCipher.

## 5. COMMANDS (Command, Purpose, Important options)
*Lưu ý: Bạn phải nhớ chính xác syntax của các lệnh này vì kỳ thi rất hay hỏi.*

*   `nslookup`: Truy vấn thông tin DNS.
    *   `set type=any` hoặc `set type=mx`: Chuyển chế độ truy vấn tìm tất cả hoặc tìm Mail server.
*   `dig`: Lệnh Linux truy vấn DNS chi tiết hơn.
    *   `dig @server_ip domain -t AXFR`: **Lệnh thực hiện DNS Zone Transfer.** (Cực kỳ quan trọng).
*   `traceroute` (Linux) / `tracert` (Windows): Đo đạc và theo dõi lộ trình đường đi của các gói tin từ máy attacker đến đích.
    *   *Mẹo ôn thi:* `traceroute` Linux mặc định dùng gói tin **UDP** hoặc TCP, `tracert` Windows sử dụng gói tin **ICMP**.
*   `ping`: Kiểm tra tính khả dụng của máy chủ đích. (Sử dụng ICMP Echo Request Type 8, Echo Reply Type 0).
*   `whois <domain>`: Tra cứu cơ sở dữ liệu Whois để lấy thông tin người đăng ký, admin email, name server.

## 6. ATTACKS & TECHNIQUES

> [!CAUTION]
> **DNS Zone Transfer (AXFR)** không hẳn là một "cuộc tấn công" mà là lỗi cấu hình (Misconfiguration). DNS Server cho phép bất kỳ ai tải xuống toàn bộ dữ liệu (zone file) thay vì chỉ cho phép Secondary DNS tải.

*   **Google Hacking (Google Dorks):** Kỹ thuật sử dụng toán tử tìm kiếm nâng cao (GHDB - Google Hacking Database).
    *   `site:` Giới hạn kết quả trong một domain cụ thể. (VD: `site:cehvietnam.com`)
    *   `intitle:` Tìm từ khóa xuất hiện trên tiêu đề trang. (VD: `intitle:"index of"`)
    *   `inurl:` Tìm từ khóa xuất hiện trong chuỗi URL. (VD: `inurl:admin login`)
    *   `filetype:` Tìm định dạng file cụ thể. (VD: `filetype:pdf password`)
*   **Website Mirroring:** Copy/Tải toàn bộ website về máy tính cục bộ để phân tích offline cấu trúc thư mục, source code ẩn (Công cụ: HTTrack, GNU Wget).
*   **Social Engineering Footprinting:** Sử dụng con người để khai thác thông tin.
    *   **Eavesdropping:** Nghe lén (thu thập âm thanh cuộc nói chuyện).
    *   **Shoulder Surfing:** Nhìn trộm qua vai (xem người khác gõ mật khẩu).
    *   **Dumpster Diving:** Bới thùng rác để tìm tài liệu nhạy cảm chứa mật khẩu, sđt, cấu trúc mạng.
    *   **Impersonation:** Giả danh (VD: giả làm nhân viên IT gọi điện yêu cầu cung cấp thông tin).
*   **Dark Web Footprinting:** Thu thập thông tin từ Dark Web (sử dụng trình duyệt Tor, các công cụ tìm kiếm củ hành như ExoneraTor) để tìm dữ liệu bị rò rỉ của công ty (Leaked DBs, Credentials).

## 7. PROTOCOLS/PORTS/SERVICES
| Protocol | Port | Công dụng trong Footprinting |
| :--- | :--- | :--- |
| **DNS** | **53 UDP** | Truy vấn tên miền (Name Resolution) thông thường. |
| **DNS** | **53 TCP** | Dành riêng cho đồng bộ dữ liệu **DNS Zone Transfer**. |
| **HTTP/HTTPS** | 80/443 | Thu thập thông tin từ Web Server, phân tích HTTP Header (Banner Grabbing). |
| **ICMP** | N/A | Giao thức nền tảng của Ping và Traceroute. Lợi dụng các thông điệp ICMP Error để vẽ bản đồ mạng. |

## 8. IMPORTANT NUMBERS & FACTS
*   **TTL (Time-To-Live):** Cốt lõi của Traceroute. Mỗi khi gói tin qua 1 router, TTL bị trừ 1. Khi TTL = 0, router sẽ loại bỏ gói tin và gửi lại thông điệp **ICMP Time Exceeded**. Nhờ đó hacker lấy được IP của các router trung gian.
*   **90%**: Là lượng thời gian một Ethical Hacker nên dành cho pha Footprinting & Reconnaissance so với tổng thời gian Pentest.
*   Bạn **không thể** ngăn chặn hoàn toàn Passive Footprinting, vì thông tin nằm trên máy chủ của bên thứ 3 (Google, Archive.org).

## 9. COMPARE & DIFFERENTIATE

### ⚖️ Passive vs Active Footprinting
> [!IMPORTANT]
> Đây là "format ôn thi tốt". Hãy phân biệt rõ 2 loại này, đề thi rất hay lừa ở ranh giới giữa Passive và Active.

| Tiêu chí | Passive Footprinting | Active Footprinting |
| :--- | :--- | :--- |
| **Bản chất** | **Không** tương tác trực tiếp mục tiêu. | **Tương tác trực tiếp** với mục tiêu. |
| **Độ rủi ro** | Gần như **bằng 0** (không bị phát hiện). | Rủi ro **cao** (cảnh báo Firewall/IDS). |
| **Ví dụ Hành động** | Tìm kiếm Google, tra Whois, dùng Archive.org, đọc báo cáo EDGAR, tìm trên Shodan. | Gõ lệnh Ping, Traceroute, Zone Transfer, Nmap scan sơ bộ, Social Engineering giả danh. |

### 📖 DNS Records (Các bản ghi DNS cần nhớ)
| Bản ghi | Ý nghĩa & Chức năng |
| :--- | :--- |
| **A** | **A**ddress: Trỏ tên miền về địa chỉ IPv4. |
| **AAAA** | Trỏ tên miền về địa chỉ IPv6. |
| **MX** | **M**ail E**x**change: Xác định Mail Server nhận email của domain. |
| **NS** | **N**ame **S**erver: Chỉ định DNS Server quản lý domain đó. |
| **CNAME** | **C**anonical **Name**: Tên bí danh (Alias), trỏ 1 tên miền sang tên miền khác. |
| **SOA** | **S**tart **O**f **A**uthority: Chứa thông tin quản trị zone (email admin, thời gian đồng bộ zone transfer). |
| **SRV** | **S**e**rv**ice: Định vị các dịch vụ đặc thù như Active Directory, SIP. |
| **PTR** | **P**oin**t**e**r**: Bản ghi dùng cho Reverse DNS (Tra từ IP ra Domain name). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Scenario-based:** Đề bài mô tả *"Hacker Bob lên website Archive.org để xem cấu trúc thư mục website của công ty X cách đây 2 năm..."* => Câu hỏi: *"Bob đang thực hiện kỹ thuật gì?"* -> **Passive Footprinting**.
*   **Scenario-based:** *"Alice gửi một yêu cầu truy vấn đến DNS Server của mục tiêu để sao chép tất cả các bản ghi..."* -> Hành động: **DNS Zone Transfer** / Công cụ: **dig -t AXFR** / Giao thức: **TCP Port 53**.
*   **Nhận diện Công cụ:** 
    *   Hỏi tool tìm IoT/SCADA -> Chọn **Shodan**.
    *   Hỏi tool chuyên trích xuất metadata từ PDF -> Chọn **FOCA**.
    *   Hỏi hệ thống lấy dữ liệu tài chính công ty (Mỹ) -> Chọn **EDGAR**.
    *   Hỏi website tìm thông tin tài khoản bị lộ ở Dark Web -> Chọn hệ thống tìm kiếm trên trình duyệt **Tor** hoặc các dịch vụ Onion.
*   **AI Tools:** Khi câu hỏi đề cập đến "sử dụng AI để tự động tạo toán tử Google Hacking" -> Chọn **DorkGPT**.

## 11. COMMON CONFUSIONS
*   **Nmap vs Recon-ng:** Đừng nhầm lẫn! Mặc dù Nmap là tool kinh điển, nó thuộc về **Scanning (Module 03)** - khi ta đã có IP và muốn quét port. Còn **Recon-ng** là tool thuộc về **Footprinting (Module 02)** dùng để thu thập OSINT, subdomain, thông tin mở.
*   **Whois vs DNS:** 
    *   **Whois:** Trả lời câu hỏi *"Ai mua tên miền này? Ngày hết hạn là bao giờ?"* (Dữ liệu con người/pháp lý).
    *   **DNS:** Trả lời câu hỏi *"Tên miền này trỏ đến IP máy chủ nào?"* (Dữ liệu kỹ thuật mạng).
*   **Shodan vs Google:** Google crawl nội dung (text, ảnh) của trang web. Shodan quét và lập chỉ mục **các cổng (ports) và banner** của thiết bị mạng.

## 12. REAL-WORLD CONTEXT
*   **GitHub Leaks:** Trong thực tế, OSINT Footprinting trên GitHub (bằng Github Dorks) cực kỳ hiệu quả. Developer thường vô tình commit mã nguồn chứa cứng (hard-code) Database Passwords, AWS API Keys lên các repo public.
*   **Social Media:** LinkedIn là "mỏ vàng" cho Footprinting. Hacker chỉ cần gõ tên công ty, xem profile nhân viên để biết công ty đang dùng công nghệ gì (Ví dụ nhân viên ghi: *"Vận hành hệ thống Cisco ASA, Windows Server 2019"* -> Lộ hệ điều hành và thiết bị).
*   **Bảo vệ DNS:** Trong thực tiễn, hầu hết các công ty hiện nay đã chặn DNS Zone Transfer. Tuy nhiên, Subdomain brute-forcing (dùng tool như Sublist3r, DNSdumpster) vẫn là cách phổ biến để tìm các máy chủ ẩn (vd: `dev.company.com`, `test.company.com`).

## 13. QUICK REVISION
1.  **Sự khác biệt cốt lõi giữa Active và Passive footprinting là gì?** -> Active có tương tác trực tiếp với đích, Passive thì thông qua bên thứ 3 (ẩn danh).
2.  **Bản ghi DNS nào phụ trách điều hướng Email?** -> MX Record.
3.  **Google Dork nào để giới hạn tìm kiếm trong một website duy nhất?** -> `site:`
4.  **Cổng và giao thức nào được sử dụng cho DNS Zone Transfer?** -> TCP Port 53.
5.  **Công cụ nào chuyên dùng để trích xuất metadata (OS, Software, Users) từ tài liệu?** -> FOCA.
6.  **Công cụ AI OSINT nào hỗ trợ sinh tự động các lệnh Google Dork phức tạp?** -> DorkGPT.

## 14. MEMORY HOOKS
*   **A**ctive = **A**ttack-like (Hành xử giống tấn công, tương tác, lưu vết log).
*   **P**assive = **P**eeking from afar (Nhìn lén từ xa, an toàn tuyệt đối).
*   **DNS Zone Transfer:** **AXFR** (Nhớ là **A**ll **X**fer = Transfer All).
*   **DNS Records:** **A** = **A**ddress (v4), **M**X = **M**ail, **N**S = **N**ame Server, **S**OA = **S**tart of Authority.
*   **FOCA:** **F**ind **O**bjects & **C**ollect **A**rchives (Chuyên moi móc dữ liệu ẩn trong file PDF/DOC).
*   **Shodan:** Con mắt thần của thế giới IoT.
