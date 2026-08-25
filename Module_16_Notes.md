# 📚 CEH v13 Study Notes - Module 16: Hacking Wireless Networks

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Hacker có thể dùng sức mạnh tính toán đám mây và AI (như Neural Networks trong Hashcat) để bẻ khóa mật khẩu WPA2 siêu nhanh. Nhưng để bắt được gói tin `4-way handshake`, bạn vẫn phải thấu hiểu nguyên lý vật lý của sóng Wi-Fi và kỹ thuật Deauth truyền thống."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân biệt 4 chuẩn mã hóa Wi-Fi: WEP (Đã chết), WPA, WPA2 (Phổ biến nhất) và WPA3 (Mới nhất, an toàn nhất). Đề thi luôn hỏi về sự tiến hóa của chúng."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Khám phá cách sóng vô tuyến (Wi-Fi) hoạt động, cách bẻ khóa các chuẩn bảo mật mạng WLAN (WEP, WPA, WPA2), và các phương pháp hack Wi-Fi hiện đại như tạo trạm phát sóng giả (Rogue AP, Evil Twin).
*   **Tại sao quan trọng:** Khác với cáp đồng, tín hiệu Wi-Fi bay lơ lửng trong không khí. Một Hacker không cần đột nhập vào công ty, chỉ cần ngồi ở quán cà phê đối diện, dùng ăng-ten định hướng là có thể hứng trọn gói tin của công ty. Nếu Wi-Fi bị hack, mạng nội bộ (LAN) coi như mở toang cửa.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **BSSID (Basic Service Set Identifier):** Địa chỉ MAC của thiết bị phát sóng Wi-Fi (Access Point - AP).
*   **SSID (Service Set Identifier):** Tên của mạng Wi-Fi hiển thị trên điện thoại/laptop của bạn (VD: "Cafe 24h").
*   **Wardriving:** Hành động lái xe chạy vòng quanh thành phố, mang theo Laptop và Ăng-ten thu sóng (GPS) để quét, định vị, và vẽ bản đồ các mạng Wi-Fi bảo mật kém.
*   **Rogue Access Point:** Cục phát Wi-Fi lậu. Nhân viên tự mang cục Wi-Fi từ nhà cắm vào mạng công ty để dùng cho tiện, vô tình tạo ra lỗ hổng vì không cài mật khẩu hoặc mã hóa yếu.
*   **Evil Twin (Sinh đôi ác kỷ):** Kẻ tấn công tạo một trạm phát Wi-Fi có *cùng tên (SSID)* và *cùng BSSID* với trạm Wi-Fi thật của công ty, nhưng phát sóng mạnh hơn. Điện thoại của nạn nhân sẽ tự động kết nối vào trạm giả này.
*   **WPS (Wi-Fi Protected Setup):** Tính năng bấm nút (hoặc nhập mã PIN 8 số) để kết nối Wi-Fi nhanh không cần nhập Pass dài. Đây là lỗ hổng chí mạng.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **WPA2 (AES - CCMP)** | Chuẩn bảo mật phổ biến nhất. Khá an toàn nhưng vẫn bẻ khóa được bằng Brute-Force nếu bắt được Handshake. |
| 🔴 **MUST MEMORIZE** | **4-Way Handshake** | Quá trình 4 bước chào hỏi giữa máy khách (Client) và Cục phát Wi-Fi (AP) để cấp chìa khóa. Bắt được nó = bẻ được Pass WPA2. |
| 🔴 **MUST MEMORIZE** | **Deauthentication Attack** | Kỹ thuật "Đá văng" nạn nhân khỏi Wi-Fi hợp pháp (Buộc họ phải kết nối lại để Hacker bắt Handshake). |
| 🟠 **HIGH PRIORITY** | **WPA3 (SAE)** | Chuẩn Wi-Fi mới nhất, chống được bẻ khóa Brute-Force (Sử dụng Simultaneous Authentication of Equals). |
| 🟠 **HIGH PRIORITY** | **Aircrack-ng** | Bộ công cụ "Thánh" của giới hack Wi-Fi. (Phải thuộc lòng). |
| 🟡 **SHOULD KNOW** | **Bluebugging** | Hack Bluetooth để chiếm quyền kiểm soát toàn bộ điện thoại (nghe gọi, đọc SMS). |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Aircrack-ng Suite** | Wi-Fi Hacker | Bộ công cụ (Airmon, Airodump, Aireplay, Aircrack) dùng để đổi card mạng sang chế độ Monitor, bắt gói tin, và bẻ khóa WEP/WPA2. | Bắt buộc phải hiểu chức năng của từng lệnh con. |
| **Kismet** | Wi-Fi Sniffer | Công cụ ngửi gói tin thụ động, chuyên dùng để phát hiện Wi-Fi ẩn (Hidden SSID), phát hiện xâm nhập (WIDS). | Làm việc cực kỳ thụ động, không ai biết. |
| **Reaver** | WPS Cracker | Chuyên đi dò mật khẩu bằng cách bẻ khóa mã PIN 8 số của tính năng WPS (Brute-force PIN). | Rất hay hỏi trong đề thi về hack WPS. |
| **Wifite** | Automated Tool | Công cụ viết bằng Python tự động hóa toàn bộ quá trình Aircrack-ng. Gõ lệnh là ngồi chơi. | Dùng trong thực chiến Kali Linux. |
| **Hashcat** | Password Cracker | Bẻ khóa file Handshake `.cap` bằng Card màn hình (GPU) cực nhanh. | Thường kết hợp với Aircrack-ng. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> Đây là Quy trình chuẩn (CEH) để hack WPA2 bằng bộ Aircrack-ng. BẠN PHẢI THUỘC LÒNG.

1.  `airmon-ng start wlan0`: Đưa card mạng từ chế độ *Managed* (chỉ nhận sóng của mình) sang chế độ **Monitor** (Thu mọi sóng trong không khí).
2.  `airodump-ng wlan0mon`: Quét toàn bộ không gian để lấy danh sách các mạng Wi-Fi và địa chỉ BSSID (MAC).
3.  `airodump-ng -c 6 --bssid [MAC_AP] -w hack wlan0mon`: Tập trung bắt gói tin của riêng cục Wi-Fi mục tiêu ở kênh (Channel) 6, lưu vào tệp `hack.cap`.
4.  `aireplay-ng -0 5 -a [MAC_AP] -c [MAC_Client] wlan0mon`: **(Tấn công Deauth)** Đá nạn nhân ra khỏi mạng 5 lần để ép nạn nhân kết nối lại. Khi đó màn hình bước 3 sẽ báo "WPA handshake captured".
5.  `aircrack-ng -w wordlist.txt hack.cap`: Bẻ khóa offline cái Handshake vừa bắt được bằng từ điển (Dictionary attack).

## 6. ATTACKS & TECHNIQUES
### Các phương pháp Hack Wi-Fi chính:
*   **Bẻ khóa WEP:** Cực kỳ lỏng lẻo do dùng mã hóa RC4 và thuật toán sinh số ngẫu nhiên IV yếu (24-bit). Chỉ cần bắt được đủ số lượng gói tin IVs (khoảng 50.000 gói) là bẻ khóa bằng toán học mất 5 giây, bất kể mật khẩu dài bao nhiêu.
*   **Bẻ khóa WPA2 (PSK):** WPA2 rất mạnh (dùng AES). Cách duy nhất để hack là bắt gói tin **4-way handshake** (khi nạn nhân nhập pass login vào mạng), sau đó mang file đó về máy nhà, lấy từ điển (wordlist) ra đập (Brute-force) cho đến khi khớp chuỗi Hash. Nếu mật khẩu mạnh -> Không thể bẻ khóa.
*   **MAC Spoofing:** Nhiều Wi-Fi nội bộ chặn người lạ bằng MAC Filter (Chỉ cho phép MAC nhân viên vào). Hacker bắt gói tin, xem MAC nào hợp lệ, rồi đổi MAC máy của Hacker thành y hệt MAC đó (Spoofing) để lọt qua trạm kiểm soát.
*   **WPS PIN Cracking:** Thay vì đoán Pass WPA2 dài ngoằng, Hacker dùng công cụ Reaver đoán mã PIN của WPS (Chỉ có 8 chữ số -> Có 10,000 tổ hợp). Rất dễ bẻ, bẻ xong WPS sẽ tự nhả ra Pass WPA2.

## 7. PROTOCOLS/PORTS/SERVICES
*   **WEP (Wired Equivalent Privacy):** Thuật toán RC4. IV = 24 bit. Chết hẳn.
*   **WPA (Wi-Fi Protected Access):** Thuật toán TKIP. Vá lỗi tạm cho WEP.
*   **WPA2:** Thuật toán CCMP (dựa trên AES mã hóa cấp quân sự). Tiêu chuẩn toàn cầu hiện nay. Lỗ hổng nổi tiếng: KRACK attack.
*   **WPA3:** Thuật toán SAE (Simultaneous Authentication of Equals). Không thể dùng đòn Deauth bắt Handshake rồi mang về Brute-force offline nữa.

## 8. IMPORTANT NUMBERS & FACTS
*   Tín hiệu Wi-Fi chia thành 2 băng tần chính: **2.4 GHz** (đi xa, xuyên tường tốt, nhiều nhiễu) và **5 GHz** (đi gần, tốc độ cao, ít nhiễu).
*   Tính năng **WPA-Enterprise (802.1X):** Trong doanh nghiệp lớn, người ta không dùng 1 pass chung cho cả cty (PSK - Pre-shared Key) mà dùng WPA-Enterprise. Mỗi nhân viên đăng nhập Wi-Fi bằng Username và Password riêng (Liên kết tới máy chủ RADIUS).

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Wardriving | Warchalking |
| :--- | :--- | :--- |
| **Hành động** | Dùng ô tô chở theo thiết bị để **quét và lưu bản đồ** mạng Wi-Fi (có GPS). | Dùng phấn **vẽ ký hiệu** lên tường/cột điện ngoài phố để đánh dấu cho Hacker khác biết khu vực đó có Wi-Fi miễn phí hoặc yếu. |

| Lỗ hổng Bluetooth | Chi tiết (Exam hay hỏi) |
| :--- | :--- |
| **Bluejacking** | Trò đùa. Gửi tin nhắn Spam (châm chọc, quảng cáo) ẩn danh qua Bluetooth. Gây phiền nhiễu, không nguy hiểm. |
| **Bluesnarfing** | Đánh cắp thông tin (Trộm danh bạ, tin nhắn, ảnh) qua Bluetooth. Nguy hiểm. |
| **Bluebugging** | Chiếm quyền điều khiển (Nghe gọi, nhắn tin bằng tiền của nạn nhân, bật mic nghe lén). Nguy hiểm nhất. |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Quy trình bẻ khóa WPA2:** Đề thi chắc chắn sẽ hỏi thứ tự các bước hoặc công cụ cụ thể: Đặt card mạng sang Monitor (`airmon-ng`) -> Bắt gói tin (`airodump-ng`) -> Gửi Deauth (`aireplay-ng`) -> Bẻ khóa offline (`aircrack-ng`).
*   **Bảo vệ Wi-Fi Doanh nghiệp:** Đề hỏi: *"Phương thức bảo mật Wi-Fi nào an toàn nhất để triển khai cho môi trường Doanh nghiệp lớn có hàng nghìn nhân viên?"* -> ĐÁP ÁN: **WPA2/WPA3 Enterprise (Dùng EAP/RADIUS server 802.1X)**. Tuyệt đối không chọn PSK.
*   **Kismet:** Nếu câu hỏi đề cập đến *"Sniffer mạng không dây thụ động, có thể phát hiện cả trạm phát sóng đã ẩn tên (Hidden SSID)"* -> Chọn **Kismet**.
*   **Rogue AP vs Evil Twin:** Rogue AP là trạm lậu do nội bộ cắm bậy vào mạng LAN (bất kỳ tên gì). Evil Twin là trạm giả do Hacker cố tình dựng lên sao chép y chang tên trạm gốc để lừa khách (VD: Giả mạo Wi-Fi quán Highlands).

## 11. COMMON CONFUSIONS
*   **Monitor Mode vs Promiscuous Mode:** 
    *   *Promiscuous Mode:* Dùng cho mạng có dây (Wired/Ethernet) để nghe lén (Module 08).
    *   *Monitor Mode:* Chỉ dành riêng cho card mạng Không dây (Wireless/Wi-Fi) để nghe mọi kênh tần số trong không khí mà không cần kết nối vào AP nào cả.
*   **WPA2 vs WPA3 Brute-force:** WPA2 bắt tay 4 bước -> Bị Brute-force offline. WPA3 dùng thuật toán SAE (Dragonfly) chặn hoàn toàn tấn công tự đoán mật khẩu offline (Nếu đoán sai vài lần kết nối sẽ bị hủy).

## 12. REAL-WORLD CONTEXT
*   **Hack Wi-Fi hàng xóm (Thực tế):** Trong thực tế, việc bẻ khóa WPA2 (Bằng Hashcat) tốn cực kỳ nhiều thời gian nếu pass dài (trên 10 ký tự, có chữ hoa, số). Hacker thường chọn con đường thứ 2 dễ hơn: Dùng **Evil Twin Attack (Fluxion/Wifiphisher)**. Hacker đá văng nạn nhân khỏi Wi-Fi thật. Dựng 1 Wi-Fi giả mạo không pass. Nạn nhân bấm vào, màn hình điện thoại tự mở trang Web giả mạo router ghi là: *"Firmware nâng cấp, vui lòng nhập mật khẩu Wi-Fi cũ để tiếp tục"*. Nạn nhân nhập pass thật -> Hacker nhận được pass dưới dạng Text mà không cần bẻ khóa 1 giây nào.

## 13. QUICK REVISION
1.  **Lỗ hổng nào của Router cho phép kết nối nhanh bằng mã PIN 8 số, rất dễ bị hack bằng tool Reaver?** -> WPS (Wi-Fi Protected Setup).
2.  **Kỹ thuật ngắt kết nối cưỡng bức nạn nhân khỏi cục phát sóng để bắt gói tin Handshake gọi là gì?** -> Deauthentication attack.
3.  **Hành động kẻ tấn công dựng một cục phát sóng giả có Tên (SSID) y hệt cục phát sóng thật gọi là gì?** -> Evil Twin.
4.  **Tấn công Bluetooth nào cho phép kẻ tấn công chiếm toàn quyền điều khiển điện thoại?** -> Bluebugging.
5.  **Thuật toán mã hóa AES được tích hợp sâu vào chuẩn bảo mật Wi-Fi nào?** -> WPA2 (CCMP).
6.  **Công cụ (bước) nào của bộ Aircrack dùng để bắt gói tin trong không khí?** -> airodump-ng.

## 14. MEMORY HOOKS
*   **Air-suite:**
    *   `airmon` = **Mon**itor (Cài đặt tầm nhìn).
    *   `airodump` = **Dump** (Bắt và nhét vào túi).
    *   `aireplay` = **Replay/Reply** (Tương tác/Đá văng).
    *   `aircrack` = **Crack** (Đập vỡ ổ khóa).
*   **Bluetooth Attacks (S-B):** Spam (Jacking) -> Steal (Snarfing) -> Takeover (Bugging). Chữ **Bug** (Lỗi/Sâu) lọt vào tận trong máy là ăn sâu nhất.
