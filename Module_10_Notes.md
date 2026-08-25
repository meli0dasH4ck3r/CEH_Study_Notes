# 📚 CEH v13 Study Notes - Module 10: Denial-of-Service

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Trong CEH v13, AI tạo ra các chiến dịch AI-Driven DDoS: Mạng Botnet có thể tự phân tích cách Firewall đang chặn để tự đổi IP, thay đổi tần suất gửi gói tin. Nhưng nguyên lý làm sập máy chủ vẫn nằm ở **TCP SYN Flood** hoặc cạn kiệt băng thông."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân loại 3 tầng DoS: Volumetric (Tầng mạng - Nghẽn băng thông), Protocol (Tầng giao thức - Cạn kiệt tài nguyên TCP), Application (Tầng ứng dụng - Chết Web/DB)."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Cách thức làm tê liệt hệ thống, khiến người dùng hợp pháp (Legitimate users) không thể truy cập dịch vụ, website hoặc hệ thống mạng. Tìm hiểu các kỹ thuật làm cạn kiệt băng thông, CPU, RAM của máy chủ.
*   **Tại sao quan trọng:** So với việc lấy cắp dữ liệu, DoS/DDoS là loại tấn công "chí mạng" nhắm vào **tính Sẵn sàng (Availability)** trong tam giác bảo mật CIA. Các công ty thương mại điện tử có thể mất hàng triệu đô la mỗi phút nếu website bị đánh sập.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **DoS (Denial-of-Service):** Tấn công từ chối dịch vụ. Thường là 1 máy tấn công 1 máy. Dễ bị chặn bằng cách block IP.
*   **DDoS (Distributed Denial-of-Service):** Tấn công từ chối dịch vụ phân tán. Hacker dùng hàng ngàn máy bị nhiễm mã độc (Botnet) để đồng loạt đánh vào 1 mục tiêu. Cực kỳ khó chặn vì IP đến từ khắp nơi trên thế giới.
*   **Botnet:** Mạng lưới "Máy tính ma" (Zombies) bị nhiễm mã độc, chịu sự điều khiển của 1 máy chủ trung tâm (C&C Server) do Hacker cầm đầu.
*   **Amplification / Reflection Attack:** Tấn công khuếch đại / Phản xạ. Hacker giả mạo IP nguồn (thành IP của nạn nhân), gửi 1 yêu cầu cực nhỏ đến các máy chủ bên thứ 3 (như DNS, NTP), các máy chủ này sẽ trả về gói tin cực lớn dội thẳng vào đầu nạn nhân. (Hệ số khuếch đại có thể lên tới 556 lần đối với NTP).

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **SYN Flood** | Gửi hàng vạn gói tin TCP SYN nhưng không bao giờ gửi ACK cuối. Máy chủ đợi mỏi mòn -> Treo. |
| 🔴 **MUST MEMORIZE** | **Botnet / Zombie** | Quân cờ trong tay hacker để tạo ra DDoS. Giao tiếp qua C&C Server. |
| 🔴 **MUST MEMORIZE** | **Amplification** | Dùng IP giả mạo gửi request nhỏ (UDP), bắt nạn nhân nhận response lớn. |
| 🟠 **HIGH PRIORITY** | **Slowloris** | Tấn công tầng ứng dụng (Application Layer). Mở kết nối HTTP và cố tình giữ nó sống lâu nhất có thể bằng cách gửi dữ liệu cực kỳ chậm. |
| 🟠 **HIGH PRIORITY** | **Ping of Death** | Kỹ thuật cũ, gửi gói tin ICMP có kích thước vượt quá giới hạn tối đa (> 65535 bytes) khiến hệ điều hành treo. |
| 🟡 **SHOULD KNOW** | **Smurf Attack** | Kỹ thuật phản xạ ICMP, gửi gói Ping đến địa chỉ Broadcast của mạng với IP giả của nạn nhân. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **HOIC / LOIC** | DoS Tool | Công cụ kinh điển (High/Low Orbit Ion Cannon) để xả gói tin HTTP, TCP, UDP tạo DDoS. | LOIC làm lộ IP thật. HOIC mạnh hơn. |
| **Hping3** | Packet Crafter | Dùng lệnh `hping3 -S --flood -V <IP>` để tạo cuộc tấn công TCP SYN Flood cực mạnh. | Tool must-know để sinh gói tin dị thường. |
| **Slowloris** | App-Layer DoS | Giữ kết nối HTTP mở với Web server (như Apache) bằng cách gửi header nhỏ giọt. Làm nghẽn hàng đợi xử lý. | Không cần băng thông lớn, vẫn đánh sập server web. |
| **RUDY** | App-Layer DoS | R-U-Dead-Yet: Tấn công bằng cách gửi dữ liệu qua Form POST với tốc độ siêu chậm. | Chuyên đánh sập Web forms. |
| **Cloudflare / Akamai** | DDoS Protection | Dịch vụ WAF/Anti-DDoS đám mây, hấp thụ toàn bộ lưu lượng tấn công trước khi nó tới Server của bạn. | Công cụ phòng thủ (Blue Team). |
| **AI-Botnets** | **AI-Powered** | Mạng lưới Botnet dùng Machine Learning để nhận diện xem WAF đang lọc IP theo pattern nào, từ đó tự đổi pattern để lách luật. | Vũ khí DDoS cực kỳ nguy hiểm trong CEHv13. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> CEH exam rất hay hỏi cấu trúc lệnh Hping3 dùng cho DoS.

*   `hping3 -S <IP> -a <Fake_IP> -p 80 --flood`: Thực hiện **TCP SYN Flood** vào cổng 80, với địa chỉ IP giả mạo (Spoofed IP) và tốc độ tối đa (`--flood`).
*   `hping3 --udp <IP> --flood`: Tấn công UDP Flood.
*   `hping3 --icmp <IP> --flood`: Tấn công ICMP Flood (Ping Flood).

## 6. ATTACKS & TECHNIQUES
### Phân loại các nhóm tấn công DoS/DDoS (3 Nhóm Cốt Lõi):
1.  **Volumetric Attacks (Tấn công băng thông - Layer 3/4):**
    *   Mục tiêu: Đổ rác vào ống nước cho đến khi ống tắc.
    *   Đơn vị đo: **Bps (Bits per second)** hoặc Gbps/Tbps.
    *   Ví dụ: UDP Flood, ICMP Flood, Amplification (NTP/DNS).
2.  **Protocol Attacks (Tấn công giao thức - Layer 3/4):**
    *   Mục tiêu: Khai thác nhược điểm của giao thức TCP/IP, làm cạn kiệt tài nguyên xử lý (CPU/RAM/Bảng trạng thái Firewall) của máy chủ.
    *   Đơn vị đo: **Pps (Packets per second)**.
    *   Ví dụ: SYN Flood, Ping of Death, Smurf, Fraggle.
3.  **Application Layer Attacks (Tấn công tầng ứng dụng - Layer 7):**
    *   Mục tiêu: Nhắm vào các lỗi của phần mềm (Web, Database) để làm treo ứng dụng. Không cần băng thông lớn, trông rất giống luồng truy cập thật.
    *   Đơn vị đo: **Rps (Requests per second)**.
    *   Ví dụ: HTTP GET Flood, Slowloris, RUDY, DNS Query Flood.

## 7. PROTOCOLS/PORTS/SERVICES
> [!CAUTION]
> Tấn công **Khuếch đại (Amplification)** chủ yếu dựa vào các giao thức **UDP**. Vì UDP "không trạng thái", dễ dàng làm giả IP nguồn (IP Spoofing) mà không bị lộ.
*   **NTP (Port 123 UDP):** Lệnh `monlist` trả về danh sách 600 máy chủ tương tác gần nhất -> Khuếch đại cực lớn (lên tới 500x).
*   **DNS (Port 53 UDP):** Gửi 1 câu truy vấn `ANY` cực ngắn, DNS Server trả về 1 cục danh sách IP siêu dài về phía nạn nhân.
*   **Memcached (Port 11211 UDP):** Đỉnh cao của Amplification, hệ số khuếch đại có thể lên tới 51,000x! (Đã tạo ra vụ DDoS lịch sử năm 2018).

## 8. IMPORTANT NUMBERS & FACTS
*   **Cơ chế bảo vệ SYN Flood (SYN Cookies):** Thay vì lưu thông tin vào RAM khi nhận SYN, Server mã hóa thông tin đó và gửi ngược lại trong gói SYN/ACK. Chỉ khi nhận được gói ACK cuối cùng, Server mới giải mã và cấp phát RAM.
*   **Băng thông kỷ lục:** Các cuộc tấn công DDoS lớn nhất hiện nay đã vượt qua mốc **3 Tbps** (Terabits per second). Các Firewall cứng truyền thống không thể chịu nổi, bắt buộc phải dùng CDN/Cloud Protection.

## 9. COMPARE & DIFFERENTIATE
| Kỹ thuật | Smurf Attack | Fraggle Attack |
| :--- | :--- | :--- |
| **Giao thức dùng** | **ICMP** (Ping) | **UDP** |
| **Cách hoạt động** | Giả IP nạn nhân, gửi Ping tới địa chỉ IP Broadcast. | Giả IP nạn nhân, gửi UDP (Port 7/19) tới Broadcast. |
| **Hậu quả** | Mọi máy trong mạng dội ngược Echo Reply về đầu nạn nhân. | Mọi máy dội ngược UDP reply về nạn nhân. |

| Kỹ thuật | Slowloris | RUDY (R-U-Dead-Yet) |
| :--- | :--- | :--- |
| **Tầng tấn công** | Lớp 7 (HTTP) | Lớp 7 (HTTP) |
| **Cách hoạt động** | Mở kết nối bằng **HTTP GET** nhưng không bao giờ gửi hết (nhỏ giọt). | Gửi dữ liệu qua **HTTP POST** form (chậm từng byte một). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **SYN Flood:** Câu hỏi: *"Hacker lạm dụng quá trình bắt tay 3 bước, gửi hàng vạn gói tin khởi tạo nhưng không phản hồi gói ACK cuối cùng. Đó là tấn công gì?"* -> Chọn **TCP SYN Flood**. (Biện pháp chống: **SYN Cookies**).
*   **Khuếch đại (Amplification):** Hỏi: *"Tấn công nào lạm dụng giao thức UDP, giả IP mục tiêu gửi request nhỏ để dội request lớn vào máy nạn nhân?"* -> Chọn **Amplification / Reflection Attack**. (Lưu ý: NTP, DNS, Memcached).
*   **Application DoS:** Đề hỏi: *"Kẻ tấn công không dùng nhiều băng thông mà làm kiệt sức Apache Web Server bằng các gói tin chưa hoàn chỉnh?"* -> Chọn **Slowloris**.
*   **Smurf Attack:** Đề hỏi: *"Kỹ thuật nào dùng gói tin ICMP dội vào IP Broadcast để đánh sập mạng?"* -> Chọn **Smurf**.
*   **Botnet C&C:** Hỏi: *"Các máy Zombies liên lạc với ai để nhận lệnh DDoS?"* -> Chọn **C&C (Command & Control) Server**.

## 11. COMMON CONFUSIONS
*   **DoS vs DDoS:** Nhiều người gộp làm 1. Nhớ kỹ: DoS là từ 1 nguồn (dễ block). DDoS là từ hàng vạn nguồn (Botnet - khó block).
*   **Volumetric vs Protocol vs Application:** 
    *   Web/App lag, Database lỗi kết nối, băng thông mạng vẫn rộng -> **Application DoS**.
    *   Firewall quá tải (Hết Connection limit) -> **Protocol DoS**.
    *   Mạng cáp quang nhà mạng sập hoàn toàn (Nghẽn cổng vật lý) -> **Volumetric DoS**.

## 12. REAL-WORLD CONTEXT
*   **Botnet IoT:** Trong thực tế, các cuộc DDoS khủng khiếp nhất không đến từ máy tính, mà đến từ hàng triệu chiếc Camera an ninh (IP Camera), Router Wi-Fi bị nhiễm mã độc (Ví dụ: **Mirai Botnet**). Chúng không có mật khẩu hoặc dùng pass mặc định (admin/admin). Mạng lưới IoT là lực lượng Zombie đông đảo nhất thế giới hiện nay.
*   **Phòng thủ:** Không một doanh nghiệp nhỏ nào tự chống được DDoS Volumetric. Phương pháp duy nhất là sử dụng **DDoS Mitigation Service** (như Cloudflare, AWS Shield) để giấu IP thật của máy chủ đi.

## 13. QUICK REVISION
1.  **Cuộc tấn công gửi lượng cực lớn lưu lượng để làm nghẹt băng thông mạng gọi là gì?** -> Volumetric Attack.
2.  **Kỹ thuật tấn công lợi dụng UDP và gửi 1 request để nhận về Response lớn gấp nhiều lần gọi là gì?** -> Amplification (Khuếch đại).
3.  **Công cụ nào giữ kết nối HTTP Web Server ở trạng thái "treo" bằng cách gửi nhỏ giọt dữ liệu?** -> Slowloris.
4.  **Tấn công khai thác lỗ hổng của TCP 3-way handshake được gọi là gì?** -> TCP SYN Flood.
5.  **Mạng lưới các máy tính bị nhiễm mã độc điều khiển bởi hacker gọi là gì?** -> Botnet.
6.  **Biện pháp kỹ thuật cốt lõi để chống lại SYN Flood ở tầng hệ điều hành?** -> Bật SYN Cookies.

## 14. MEMORY HOOKS
*   **SYN Flood:** Như một người gọi điện thoại cấp cứu, tổng đài nhấc máy (SYN/ACK), nhưng người gọi để ống nghe đó đi chơi (Không thèm ACK). Tổng đài bị treo line.
*   **Smurf:** (Tiếng Việt: Xì trum). Lấy loa hò hét vào một ngôi làng Xì trum (Broadcast), giả vờ mình là Tí (IP nạn nhân). Cả làng đồng thanh gào lại tên Tí. Tí bị điếc (DoS).
*   **DDoS = D**istributed (Phân tán - Đến từ khắp mọi nơi).
