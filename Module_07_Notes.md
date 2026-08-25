# 📚 CEH v13 Study Notes - Module 07: Malware Threats

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** CEH v13 cảnh báo sự xuất hiện của AI-Generated Malware (Mã độc sinh bởi AI như ChatGPT, WormGPT), chúng thay đổi chữ ký (signature) liên tục. Tuy nhiên, nếu bạn hiểu hành vi lõi (kết nối C2, sửa Registry), bạn vẫn sẽ phát hiện ra chúng."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân loại 4 loại mã độc chính: Trojan, Virus, Worm, Ransomware. Đề thi sẽ hỏi các tình huống thực tế để bạn phân biệt chúng."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Khái niệm, cách thức hoạt động, cách lây lan và cách phân tích các loại phần mềm độc hại (Malware) bao gồm Trojan, Virus, Worm, Ransomware, Botnet, Fileless Malware.
*   **Tại sao quan trọng:** Malware là công cụ đắc lực nhất (Vũ khí) của Hacker ở giai đoạn *Maintaining Access* (Duy trì truy cập) và phá hoại hệ thống. Hiểu Malware là hiểu cách phòng thủ chống lại các chiến dịch tống tiền (Ransomware) và gián điệp (APT).

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Malware (Malicious Software):** Phần mềm độc hại, được thiết kế để phá hoại, đánh cắp dữ liệu, hoặc kiểm soát hệ thống trái phép.
*   **C&C Server (Command & Control):** Máy chủ điều khiển và ra lệnh cho các thiết bị đã bị nhiễm mã độc (như mạng Botnet).
*   **Crypter / Packer:** Công cụ mã hóa hoặc nén Malware gốc để che giấu chữ ký (Signature) nhằm qua mặt phần mềm diệt virus (AV Evasion). Bề ngoài file trông khác đi, nhưng khi chạy lên RAM, nó sẽ tự giải mã và hoạt động.
*   **Fileless Malware:** Mã độc "không tệp". Nó không để lại file `.exe` trên ổ cứng mà chạy thẳng vào RAM (Memory) bằng các công cụ hợp lệ của Windows (như PowerShell, WMI) để tránh bị AV quét ổ cứng phát hiện.
*   **APT (Advanced Persistent Threat):** Mối đe dọa dai dẳng nâng cao. Thường là các nhóm hacker quốc gia ẩn nấp trong mạng hàng tháng, hàng năm để do thám.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Trojan** | Giả vờ là phần mềm hợp pháp (ví dụ: file PDF, game crack) để dụ người dùng click. **Không tự lây lan.** |
| 🔴 **MUST MEMORIZE** | **Virus** | Lây nhiễm vào các file hợp pháp. **Cần người dùng kích hoạt** (click vào file). |
| 🔴 **MUST MEMORIZE** | **Worm (Sâu máy tính)** | Lây lan qua mạng **hoàn toàn tự động**, không cần con người tương tác. Ăn băng thông. |
| 🔴 **MUST MEMORIZE** | **Ransomware** | Mã hóa toàn bộ ổ cứng và đòi tiền chuộc (Bitcoin). Nỗi khiếp sợ của doanh nghiệp. |
| 🟠 **HIGH PRIORITY** | **Botnet** | Mạng lưới các "máy tính ma" bị nhiễm mã độc, chịu sự điều khiển của 1 C&C Server (để DDoS). |
| 🟠 **HIGH PRIORITY** | **Sheep Dip** | Máy tính bị cô lập hoàn toàn khỏi mạng lưới, chuyên dùng để kiểm tra mã độc. |
| 🟡 **SHOULD KNOW** | **Polymorphic Virus** | Virus đa hình: Tự đổi mã nguồn (signature) sau mỗi lần lây nhiễm để trốn AV. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **NJRat / DarkComet** | RAT (Remote Access Trojan) | Cho phép hacker điều khiển máy tính nạn nhân từ xa (quay lén webcam, keylogger, chụp màn hình). | Công cụ kinh điển tạo ra Trojan. |
| **VirusTotal** | Static Analysis | Web upload file/hash để quét qua hơn 70 engine diệt virus toàn cầu xem có độc không. | Dùng trong phân tích tĩnh. |
| **Any.Run / Cuckoo Sandbox** | Dynamic Analysis | Sandbox trực tuyến (chạy file độc hại trong môi trường ảo) để xem hành vi của nó (kết nối IP nào, tạo file gì). | Dùng trong phân tích động. |
| **Process Monitor (Sysinternals)** | Malware Analysis | Theo dõi thời gian thực (real-time) xem Malware đã sửa Registry hay File nào. | Tool cực kỳ mạnh của Microsoft. |
| **WormGPT / FraudGPT** | **AI-Powered** | Các mô hình LLM (như ChatGPT) nhưng bị gỡ bỏ bộ lọc đạo đức. Hacker dùng để viết Malware siêu tốc và không có lỗi chính tả (lừa đảo Email cực giỏi). | AI làm vũ khí. |
| **Deep Instinct / Cylance** | **AI-Powered AV** | Phần mềm chống mã độc thế hệ mới (EDR). Dùng AI/Deep Learning để bắt Malware "mới toanh" (Zero-day) thông qua hành vi, thay vì dùng Signature cũ. | AI làm khiên đỡ. |

## 5. COMMANDS (Command, Purpose, Important options)
*   Malware thường lợi dụng các lệnh hệ thống để sống sót (Persistence):
    *   `reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Backdoor /t REG_SZ /d "C:\malware.exe"`: Lệnh thêm Malware vào Registry khởi động cùng Windows.
    *   `powershell.exe -nop -w hidden -enc <Base64_Payload>`: Kỹ thuật kinh điển của **Fileless Malware**, gọi PowerShell chạy ngầm một đoạn mã hóa Base64 mà không tải file về ổ cứng.

## 6. ATTACKS & TECHNIQUES
*   **Quy trình phân tích mã độc (Malware Analysis):**
    1.  **Static Analysis (Phân tích tĩnh):** Không chạy file. Dịch ngược (Reverse Engineering), kiểm tra Hash, đọc chuỗi (Strings), xem file dùng thư viện (DLL) gì. An toàn nhưng khó biết hành vi thật nếu file bị Packed.
    2.  **Dynamic Analysis (Phân tích động):** Phải chạy file trong môi trường cô lập (Sandbox). Giám sát hành vi tạo Process, sửa Registry, gửi gói tin DNS/HTTP ra ngoài.
*   **Wrappers / Binders:** Kỹ thuật ghép (bind) 1 file mã độc (backdoor.exe) chung với 1 file hợp pháp (Mario.exe). Khi nạn nhân click, Mario vẫn chơi bình thường, nhưng backdoor chạy ngầm phía sau.
*   **Obfuscation (Làm rối mã):** Đảo lộn mã nguồn phần mềm để các nhà phân tích tĩnh khó đọc hiểu.

## 7. PROTOCOLS/PORTS/SERVICES
*   **DNS & HTTP/HTTPS:** Malware hiện đại luôn trốn traffic gọi về nhà (Call-home C&C) qua cổng 80 và 443 vì tường lửa công ty hiếm khi chặn 2 cổng này.
*   **IRC (Port 6667):** Giao thức chat cổ điển, từng được dùng rất nhiều để điều khiển mạng Botnet.

## 8. IMPORTANT NUMBERS & FACTS
*   **WannaCry (2017):** Ransomware nổi tiếng nhất lịch sử. Nó nguy hiểm vì kết hợp tống tiền với cơ chế lây lan tự động của **Worm** (Khai thác lỗ hổng EternalBlue - SMB Port 445 của Windows).
*   Các virus lây nhiễm File thường sửa **OEP (Original Entry Point)** của file `.exe`. Khi chạy, chương trình sẽ nhảy sang chạy mã độc trước, rồi mới nhảy về chạy file thật.

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Virus | Worm (Sâu) | Trojan |
| :--- | :--- | :--- | :--- |
| **Cách lây lan** | Bám vào file hợp lệ. **Cần người dùng click**. | Tự động quét IP mạng và tự nhân bản. | Giả vờ là phần mềm hợp pháp. |
| **Mức độ lây lan** | Chậm (trong cùng máy tính) | Cực nhanh (lan khắp toàn cầu, nghẽn mạng) | Rất ít (chủ yếu là tải Backdoor) |
| **Ví dụ** | Virus lây file Word, Excel | WannaCry, Blaster | DarkComet RAT, ZeuS |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Phân biệt loại Malware:**
    *   Đề hỏi: *"Mã độc này tự lây lan khắp mạng lưới mà không cần User tương tác"* -> **Worm**.
    *   Đề hỏi: *"Tải game lậu từ Internet, cài xong bị hacker điều khiển chuột"* -> **Trojan**.
    *   Đề hỏi: *"Phần mềm liên tục đổi mã hash sau mỗi lần lây nhiễm để trốn AV"* -> **Polymorphic Virus** (Virus đa hình).
*   **Phân tích tĩnh vs động:** Đề hỏi *"Kỹ thuật nào chạy malware trong Sandbox để theo dõi Registry changes?"* -> **Dynamic Analysis**.
*   **Fileless Malware:** Mã độc không để lại dấu vết trên đĩa, chạy hoàn toàn trong **RAM / Memory**. Cực kỳ nguy hiểm.
*   **Sheep Dip:** Là máy tính cô lập, cài công cụ phân tích để kiểm tra file đáng ngờ. (Format thi hay hỏi khái niệm này).

## 11. COMMON CONFUSIONS
*   **Trojan vs Virus:** Người thường gọi chung mọi mã độc là "Virus". Nhưng trong chuyên ngành CEH: Trojan KHÔNG nhân bản và KHÔNG lây lan sang file khác. Nó chỉ nằm im đó mở cửa sau.
*   **Crypter vs Packer:** 
    *   Crypter: Mã hóa file mã độc thành mớ hỗn độn (tàng hình).
    *   Packer: Nén file lại cho nhỏ gọn (cũng làm thay đổi Signature của file gốc).

## 12. REAL-WORLD CONTEXT
*   **Ransomware-as-a-Service (RaaS):** Ở thế giới ngầm hiện nay, các hacker giỏi lập trình sẽ tạo ra Ransomware, sau đó cho các hacker cấp thấp thuê để đi phát tán (giống như kinh doanh nhượng quyền). Tiền chuộc sẽ chia theo tỷ lệ.
*   **Macro Virus:** Rất phổ biến hiện nay. Nạn nhân nhận email trúng thưởng đính kèm file Excel. Khi mở file và bấm "Enable Content", mã Macro (VBA) sẽ tự tải mã độc về máy. (Đây là kỹ thuật mồi nhử thường gặp nhất).

## 13. QUICK REVISION
1.  **Loại mã độc nào yêu cầu người dùng phải click vào để lây nhiễm?** -> Virus.
2.  **Mã độc nào tự động nhân bản qua đường truyền mạng?** -> Worm.
3.  **Kỹ thuật phân tích nào cần phải thực thi (chạy) Malware trong môi trường ảo (Sandbox)?** -> Phân tích động (Dynamic Analysis).
4.  **Máy chủ trung tâm dùng để điều khiển hàng triệu thiết bị Botnet gọi là gì?** -> C&C Server (Command and Control).
5.  **Thuật ngữ chỉ loại Malware chạy thẳng trên RAM bằng PowerShell mà không tải file .exe xuống?** -> Fileless Malware.

## 14. MEMORY HOOKS
*   **T-V-W (Trojan, Virus, Worm):**
    *   **T**rojan = **T**ricky (Lừa đảo ngụy trang con ngựa gỗ thành Troy).
    *   **V**irus = **V**ictim (Cần nạn nhân kích hoạt).
    *   **W**orm = **W**orldwide (Bò khắp thế giới một cách tự động).
*   **C&C Server:** Cứ nhớ là **C**ommand (Ra lệnh) & **C**ontrol (Điều khiển).
