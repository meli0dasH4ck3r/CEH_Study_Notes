# 📚 CEH v13 Study Notes - Module 04: Enumeration

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** CEH v13 liên tục nhấn mạnh việc dùng GenAI (ChatGPT) để sinh prompt và tự động hóa Enumeration, nhưng nếu bạn không biết **NetBIOS code <20>** là gì, hay cổng **SMB 445** hoạt động ra sao, AI cũng vô dụng với bạn."
> *   "Với các giao thức trong phần này (NetBIOS, SNMP, LDAP, SMB), **hãy tự vẽ tay sơ đồ mindmap** phân loại Port và Công cụ tương ứng. Môn thi CEH rất thích hỏi chéo (Giao thức này dùng cổng nào, tool nào)."
> *   **Công thức tư duy:** `Scanning = Tìm cửa mở` -> `Enumeration = Lấy danh sách tài sản bên trong cửa (Users, Shares, Passwords)`.

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Enumeration (Điểm danh/Liệt kê) là quá trình thiết lập các kết nối **chủ động (active)** với mục tiêu để trích xuất danh sách người dùng (usernames), nhóm mạng (groups), thư mục chia sẻ (shares), và cấu hình dịch vụ.
*   **Tại sao quan trọng:** Đây là bước cuối cùng trước khi tấn công thật sự (System Hacking). Nếu Footprinting cho biết "nhà ở đâu", Scanning cho biết "cửa nào mở", thì Enumeration sẽ "nhìn qua khe cửa" để ghi lại "tên chủ nhà, sơ đồ phòng ốc, chìa khóa dự phòng treo ở đâu". Không có Enumeration, bạn phải tấn công mò mẫm (brute-force) mù quáng.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Enumeration:** Thu thập thông tin chi tiết (users, routing tables, machine names) thông qua kết nối mạng trực tiếp đến các dịch vụ (SMB, SNMP, LDAP...).
*   **Null Session (SMB):** Khai thác lỗi thiết kế của Windows (đời cũ), cho phép tạo kết nối ẩn danh (không cần username/password) qua cổng 139/445 để lấy danh sách người dùng và thư mục chia sẻ.
*   **SNMP Community String:** Đóng vai trò như "Mật khẩu" để truy cập dữ liệu SNMP. Mặc định cực kỳ yếu: `public` (Chỉ đọc - Read-only) và `private` (Đọc/Ghi - Read-Write).
*   **MIB (Management Information Base):** Cơ sở dữ liệu phân cấp chứa cấu hình, thống kê của các thiết bị mạng được quản lý bởi SNMP.
*   **DNS Zone Walking:** Khai thác lỗi của DNSSEC (Bản ghi NSEC) để liệt kê tất cả các tên miền con (subdomains) mà không cần Brute-force.
*   **DNS Cache Snooping:** Truy vấn máy chủ DNS để xem nó có "nhớ" (cache) một tên miền cụ thể nào đó không, từ đó suy ra hành vi lướt web hoặc hệ thống nội bộ của công ty.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Null Session** | Điểm yếu chí mạng của Windows cho phép đọc dữ liệu không cần mật khẩu. |
| 🔴 **MUST MEMORIZE** | **SNMP** | (Simple Network Management Protocol) - Mỏ vàng thông tin thiết bị mạng. |
| 🔴 **MUST MEMORIZE** | **NetBIOS Codes** | Các mã Hex 2 ký tự (vd `<20>`) mô tả chức năng của máy tính Windows. |
| 🟠 **HIGH PRIORITY** | **enum4linux** | Tool kinh điển số 1 để cào dữ liệu từ SMB/NetBIOS. |
| 🟠 **HIGH PRIORITY** | **Zone Walking** | Lỗ hổng của bản ghi NSEC trong DNSSEC. |
| 🟡 **SHOULD KNOW** | **LDAP** | (Lightweight Directory Access Protocol) - Dùng để truy vấn Active Directory. |
| 🟡 **SHOULD KNOW** | **SMTP VRFY/EXPN** | Các lệnh email dùng để xác minh người dùng có tồn tại hay không. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)

> [!TIP]
> **[AI Overlay trong CEH v13]** V13 bổ sung một khái niệm lớn: **Enumeration with AI**. Kẻ tấn công có thể dùng GenAI (như ChatGPT, Claude) để tạo ra các lệnh quét tự động, phân tích phản hồi dài (ví dụ: chuỗi MIB khổng lồ từ SNMP) và dịch nó thành danh sách lỗ hổng dễ đọc. Tuy nhiên, AI thực thi lệnh qua các công cụ truyền thống dưới đây.

| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **enum4linux / nullinux** | SMB/Samba Enum | Trích xuất user, shares, chính sách mật khẩu, OS từ Windows/Samba qua Null Sessions. | Công cụ mạnh nhất cho SMB Enumeration trên Linux. |
| **nbtstat** | NetBIOS Enum | Công cụ mặc định của Windows để tra cứu NetBIOS qua IP. | Trả về các mã Hex codes quan trọng (sẽ hỏi trong bài thi). |
| **snmpwalk** | SNMP Enum | Truy vấn thiết bị SNMP để lấy toàn bộ "cây" thư mục MIB (thông tin phần cứng, RAM, interface). | Thường kết hợp với chuỗi `public` mặc định. |
| **OpUtils** | SNMP Enum | Trình quản lý mạng có GUI để quét và vẽ sơ đồ SNMP. | Thường gặp trong đáp án nhiễu hoặc hỏi GUI tool. |
| **AD Explorer / JXplorer** | LDAP Enum | Giao diện đồ họa để duyệt và xuất cấu trúc thư mục Active Directory (LDAP). | Dùng để tìm kiếm admin users trong AD. |
| **smtp-user-enum** | SMTP Enum | Brute-force hoặc dò tên người dùng qua giao thức Email (SMTP). | Tận dụng các lệnh VRFY, EXPN, RCPT TO. |
| **showmount / rpcinfo** | NFS/RPC Enum | Xem các thư mục đang được share công khai trên hệ thống Linux (NFS). | Tìm dữ liệu nhạy cảm chia sẻ lỏng lẻo. |
| **SMBMap** | SMB Enum | Quét hàng loạt máy chủ để tìm các ổ đĩa chung (shares) và quyền truy cập (Read/Write). | Rất hay dùng thực chiến Pentest. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> CEH exam yêu cầu bạn biết lệnh nào sinh ra từ công cụ nào, và để làm gì.

*   **NetBIOS / SMB:**
    *   `nbtstat -A <IP>`: Liệt kê NetBIOS name table (tên máy, MAC) của mục tiêu dựa trên IP.
    *   `nbtstat -c`: Xem bộ nhớ cache NetBIOS nội bộ.
    *   `enum4linux -a <IP>`: `-a` (All). Quét toàn diện SMB (Users, Groups, Shares, Password Policy).
*   **SNMP:**
    *   `snmpwalk -v 2c -c public <IP>`: Dùng SNMP version 2c, với community string là `public` để trích xuất toàn bộ MIB.
*   **SMTP (Lệnh gõ tay qua Telnet vào Port 25):**
    *   `VRFY <username>`: (Verify) Hỏi server xem username này có tồn tại không. (Trả về 250 là có).
    *   `EXPN <list>`: (Expand) Hỏi server xem mailing list (vd: IT-Dept) bao gồm những email cá nhân nào.
    *   `RCPT TO:<email>`: Xác nhận địa chỉ nhận email có hợp lệ không (thường bị lợi dụng nếu VRFY bị chặn).
*   **NTP (Network Time Protocol):**
    *   `ntpdate`, `ntptrace`, `ntpdc`, `ntpq`: Các lệnh dùng để lấy thông tin đồng bộ thời gian (có thể suy ra cấu trúc mạng nội bộ).

## 6. ATTACKS & TECHNIQUES

*   **NetBIOS Enumeration:** Khai thác cổng 137, 138, 139 để lấy Tên máy tính (Computer name) và Tên miền (Domain name). Chỉ hoạt động trên mạng LAN (IPv4, không dùng được cho IPv6 trừ khi có cơ chế đặc biệt).
*   **Null Session Attack (SMB):** Khai thác IPC$ (Inter-Process Communication) share. Kẻ tấn công tạo kết nối rỗng (User = "", Password = ""). Nếu Windows cấu hình kém, Hacker sẽ sao chép được toàn bộ danh sách User và Password Policies.
*   **SNMP Enumeration:** Kẻ tấn công dò tìm Community string (ví dụ dùng công cụ Onesixone, Hydra). Khi có chuỗi này, chúng sẽ tải về sơ đồ mạng, bảng định tuyến, phiên bản OS, và số lượng máy trạm.
*   **LDAP Enumeration:** Truy vấn cổng 389 để lấy toàn bộ cây cấu trúc của công ty (sơ đồ tổ chức, danh bạ nhân sự, vị trí địa lý của máy tính). Rất nguy hiểm vì LDAP chứa thông tin bảo mật của Active Directory.

## 7. PROTOCOLS/PORTS/SERVICES (Cực kỳ quan trọng)
| Protocol | Port | Mục đích trong Enumeration |
| :--- | :--- | :--- |
| **NetBIOS** | 137 (UDP), 138 (UDP), 139 (TCP) | Phân giải tên máy tính, Datagram, và Phiên làm việc. |
| **SMB** | **445 (TCP)** | Chia sẻ file/máy in Windows, thực thi RPC. Lỗ hổng Null Session nằm ở đây. |
| **SNMP** | **161 (UDP)** | Truy vấn dữ liệu từ thiết bị mạng. (162 là Trap - thiết bị gửi cảnh báo về Server). |
| **LDAP / LDAPS** | **389 (TCP/UDP) / 636 (TCP)** | Truy vấn thư mục (Active Directory). Đóng vai trò là "Danh bạ" của mạng lưới. |
| **NTP** | **123 (UDP)** | Đồng bộ thời gian. Lộ thông tin IP nội bộ. |
| **SMTP** | **25 (TCP)**, 465, 587 | Liệt kê username, danh sách email nội bộ. |
| **RPC** | 111 (TCP/UDP), 135 (TCP) | Remote Procedure Call (Portmap). Dùng để liệt kê dịch vụ NFS, Exchange. |

## 8. IMPORTANT NUMBERS & FACTS
> [!WARNING]
> BẮT BUỘC PHẢI NHỚ CÁC MÃ NETBIOS NÀY ĐỂ THI CEH:

*   **<00>**: Tên máy trạm (Workstation Service).
*   **<20>**: Dịch vụ chia sẻ File và Máy in (File Server Service). -> Nếu thấy mã này, máy đó có share folder.
*   **<1B>**: Domain Master Browser (Máy chủ kiểm soát danh sách các máy tính trong Domain).
*   **<1C>**: Domain Controller (Máy chủ quản lý tài khoản Active Directory).
*   **<1D>**: Master Browser (Máy đứng đầu mạng LAN để lưu danh sách máy tính).

## 9. COMPARE & DIFFERENTIATE

### ⚖️ SMTP Commands: VRFY vs EXPN
| Thuộc tính | VRFY (Verify) | EXPN (Expand) |
| :--- | :--- | :--- |
| **Mục đích** | Kiểm tra xem **một** người dùng có tồn tại không. | Giải mã **một nhóm** (mailing list) thành các email cá nhân. |
| **Rủi ro** | Lộ tên đăng nhập hệ thống (brute-force user). | Lộ danh sách toàn bộ phòng ban, địa chỉ bí mật. |

### ⚖️ SNMP Versions
| Phiên bản | Đặc điểm Security | Ghi chú |
| :--- | :--- | :--- |
| **SNMP v1 / v2c** | 🔴 Kém bảo mật | Mật khẩu (Community string) được truyền bằng văn bản gốc (Clear-text). Ai sniff cũng đọc được. |
| **SNMP v3** | 🟢 An toàn | Mã hóa dữ liệu (Encryption) và Yêu cầu xác thực mạnh (Authentication). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Hỏi về mã NetBIOS:** Đề thi đưa ra một bảng kết quả `nbtstat`, hỏi *"Đâu là File Server?"* -> Tìm dòng kết thúc bằng `<20>`. Hỏi *"Đâu là Domain Controller?"* -> Tìm `<1C>`.
*   **Hỏi về SNMP:** Đề mô tả một giao thức quản lý mạng truyền mật khẩu dưới dạng plaintext, dùng UDP 161. Hỏi giao thức và giải pháp? -> Đáp án: SNMP v1/v2c. Giải pháp là nâng cấp lên SNMP v3.
*   **Lệnh SMTP:** Đề thi hỏi lệnh Telnet nào dùng để bung một nhóm email thành nhiều email con -> Đáp án: **EXPN**.
*   **Hỏi về DNS Zone Walking:** Kỹ thuật lợi dụng bản ghi nào để liệt kê DNS mà không cần Brute-force -> Đáp án: **Bản ghi NSEC (của DNSSEC)**.
*   **Hỏi về Port:** Hỏi cổng nào hỗ trợ LDAP an toàn có mã hóa (LDAPS) -> Đáp án: **636**.
*   **AI trong v13:** Nếu đề bài có nhắc *"Sử dụng LLM, ChatGPT để sinh prompt truy vấn SMB/NetBIOS tự động"*, hãy nhớ đây là tính năng mới (Enumeration using AI).

## 11. COMMON CONFUSIONS
*   **Scanning vs Enumeration:** 
    *   Scanning: Đi dọc hành lang khách sạn, thử xoay nắm đấm cửa xem phòng nào không khóa (Port scan). 
    *   Enumeration: Đẩy cửa bước vào phòng không khóa, ghi chép lại trong phòng có cái gì, giấy tờ trên bàn ghi tên ai (Liệt kê User, Share, Password).
*   **Zone Transfer vs Zone Walking:**
    *   Zone Transfer (AXFR): Lỗi do admin cấu hình sai, cho copy toàn bộ.
    *   Zone Walking: Lỗi thiết kế của công nghệ bảo mật DNSSEC (dùng NSEC), kẻ tấn công lấy được chuỗi các subdomain được mã hóa theo thứ tự bảng chữ cái.

## 12. REAL-WORLD CONTEXT
*   **SMBv1 và Null Session:** Null Session gần như đã tuyệt chủng trên Windows 10 và Server 2016+ vì SMBv1 bị vô hiệu hóa mặc định. Tuy nhiên, trong mạng nội bộ doanh nghiệp, vẫn còn hằng hà sa số các thiết bị cũ, máy in, máy quét y tế chạy Windows 7 / XP chứa lỗi này.
*   **NFS (Network File System):** Nhiều lập trình viên Linux vô tình cấu hình chia sẻ thư mục gốc `/` qua NFS với quyền ẩn danh (`*`). Dùng `showmount -e` sẽ thấy ngay. Kẻ tấn công có thể mount thư mục đó về máy mình và đọc file cấu hình hệ thống.
*   **SNMP Default Strings:** Rất nhiều Switch/Router công nghiệp được lắp đặt nhưng IT không đổi chữ `public` và `private`. Dùng `snmpwalk`, hacker có thể lấy được toàn bộ mật khẩu Admin dạng Hash.

## 13. QUICK REVISION
1.  **Lỗ hổng nào của SMB cho phép kết nối ẩn danh mà không cần mật khẩu?** -> Null Session.
2.  **Cổng mạng cho LDAP và LDAPS lần lượt là bao nhiêu?** -> 389 và 636.
3.  **Công cụ `enum4linux` chuyên dùng để lấy dữ liệu từ giao thức nào?** -> SMB / NetBIOS.
4.  **Lệnh SMTP nào dùng để mở rộng mailing list?** -> EXPN.
5.  **SNMP v3 khác gì v2?** -> v3 hỗ trợ Mã hóa (Encryption) và Xác thực (Authentication).
6.  **Mã NetBIOS `<20>` có nghĩa là gì?** -> File Server (Có chia sẻ thư mục).

## 14. MEMORY HOOKS
*   **NetBIOS Mnemonic:** 
    *   `<00>`: 00 là số 0, là điểm bắt đầu -> Trạm làm việc (Workstation).
    *   `<20>`: Điểm 10/10 x 2 = 20 -> Ngon nhất vì có File chia sẻ (File Share).
    *   `<1C>`: Chữ **C** -> Domain **C**ontroller.
*   **SNMP Ports:** 16**1** = **1**ndividual (Client hỏi server, udp 161). 16**2** = **2** you (Server tự gửi cảnh báo Trap về cho bạn, udp 162).
*   **SMTP VRFY vs EXPN:** **V**RFY = **V**erify (1 người). **E**XPN = **E**xpand (Bung lụa thành nhiều người).
