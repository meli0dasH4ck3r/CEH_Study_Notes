# 📚 CEH v13 Study Notes - Module 17: Hacking Mobile Platforms

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Hacker dùng AI để phân tích mã nguồn (Reverse Engineering) các app Android/iOS cực nhanh, hoặc dùng Deepfake lừa xác thực khuôn mặt sinh trắc học. Tuy nhiên, kiến trúc Sandboxing của iOS hay Android Sandbox mới là cốt lõi để bảo vệ thiết bị, và Root/Jailbreak chính là hành động phá vỡ lớp vỏ đó."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** So sánh cấu trúc bảo mật giữa Android (Mở) và iOS (Đóng). Bạn phải hiểu được ý nghĩa của Rooting và Jailbreaking khác nhau thế nào trong môi trường Enterprise (MDM)."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Nghiên cứu kiến trúc bảo mật của hệ điều hành di động (Android, iOS). Các phương thức tấn công thiết bị di động như: Ứng dụng độc hại, hack qua Bluetooth/Wi-Fi công cộng, vượt rào (Root/Jailbreak), và các giải pháp quản lý thiết bị di động trong doanh nghiệp (MDM/BYOD).
*   **Tại sao quan trọng:** Thế giới đang dịch chuyển sang Mobile-First. Một chiếc điện thoại hiện nay chứa toàn bộ mã OTP ngân hàng, email công việc, thông tin sinh trắc học và vị trí định vị 24/7. Nếu xâm nhập được điện thoại, Hacker có được một "máy bay không người lái" hoàn hảo để do thám và tấn công mạng lõi công ty.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Android Sandbox:** Cơ chế cách ly (Cô lập). Mỗi ứng dụng trên Android chạy trong một "hộp cát" riêng với 1 User ID (UID) riêng biệt. App A không thể đọc trộm dữ liệu của App B trừ khi được cấp quyền (Permissions).
*   **Rooting (Android):** Hành động can thiệp vào HĐH Android để giành quyền cao nhất (Root/Superuser). Nó phá vỡ hoàn toàn cơ chế Sandbox, cho phép 1 App độc hại xóa hoặc sửa mọi file hệ thống.
*   **Jailbreaking (iOS):** Tương tự Rooting nhưng áp dụng cho hệ sinh thái khép kín của Apple. Hành động này gỡ bỏ các rào cản phần mềm của Apple, cho phép người dùng cài đặt ứng dụng nằm ngoài App Store (Cydia/Sileo).
*   **OWASP Mobile Top 10:** Danh sách 10 lỗ hổng nghiêm trọng nhất dành riêng cho ứng dụng Di động (Khác với bản Web).
*   **BYOD (Bring Your Own Device):** Xu hướng Doanh nghiệp cho phép nhân viên mang điện thoại cá nhân (chứa game, app linh tinh) kết nối vào mạng công ty để làm việc. (Đây là cơn ác mộng của quản trị viên).

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **MDM (Mobile Device Management)** | Giải pháp quản trị thiết bị di động của Doanh nghiệp (Xóa dữ liệu từ xa, cấm dùng camera, cấm Root). |
| 🔴 **MUST MEMORIZE** | **APK / IPA** | Đuôi file bộ cài ứng dụng. APK = Android, IPA = iOS. Cực kỳ hay hỏi. |
| 🔴 **MUST MEMORIZE** | **Sandboxing** | Cơ chế phòng thủ cốt lõi của cả iOS và Android. |
| 🟠 **HIGH PRIORITY** | **Sideloading** | Hành động cài đặt ứng dụng từ nguồn bên ngoài (Web) thay vì tải từ App Store / Google Play chính thống. |
| 🟠 **HIGH PRIORITY** | **Reverse Engineering** | Dịch ngược mã nguồn app (Từ đuôi .apk về ngôn ngữ Java) để tìm mật khẩu (Hardcoded pass). |
| 🟡 **SHOULD KNOW** | **Smishing** | Tấn công gửi tin nhắn SMS chứa link tải mã độc (Thường nhắm vào thiết bị Mobile). |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **MobSF** | Mobile Security Framework | (Mobile Security Framework). Nền tảng tự động quét, phân tích mã nguồn (Tĩnh và Động) cho cả file APK (Android) và IPA (iOS). | Tool "quốc dân" cho Mobile Pentest. |
| **Apktool** | Reverse Engineering | Dịch ngược file `.apk` (Android) ra các file mã nguồn XML và cấu trúc thư mục, cho phép Hacker sửa code rồi đóng gói lại. | Công cụ tháo/lắp app cơ bản. |
| **Drozer** | Android Testing | Khung kiểm thử toàn diện chuyên tìm các lỗ hổng giao tiếp (IPC) giữa các app trên Android. | Rất mạnh, hay thi. |
| **Frida / Objection** | Dynamic Instrumentation | Tiêm mã trực tiếp vào ứng dụng Mobile *đang chạy* (trên RAM) để qua mặt bước kiểm tra bảo mật (Bypass SSL Pinning, Root Detection). | Công cụ của Pentester chuyên nghiệp. |
| **AI Deepfakes** | **AI-Powered** | Kẻ tấn công dùng AI tạo video giả khuôn mặt/giọng nói để qua mặt hệ thống eKYC (xác thực sinh trắc học mở tài khoản ngân hàng trên app di động). | Ứng dụng AI rất nguy hiểm trong v13. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!NOTE]
> CEH exam chủ yếu kiểm tra lý thuyết về kiến trúc Mobile, ít hỏi lệnh Console hơn các module khác. Tuy nhiên, cần biết công cụ dịch ngược (Decompiling).

*   **Apktool (Dịch ngược & Đóng gói):**
    *   `apktool d app.apk`: Dịch ngược (Decompile) ứng dụng ra thành thư mục.
    *   `apktool b thu_muc`: Đóng gói (Build) thư mục trở lại thành tệp `.apk` mới. (Sau đó phải dùng `jarsigner` để ký chữ ký điện tử thì máy mới cho chạy).

## 6. ATTACKS & TECHNIQUES
### Phân loại các vectơ tấn công Mobile:
1.  **Thiết bị vật lý (Device Level):**
    *   Mất cắp thiết bị (Mất trắng dữ liệu nếu không mã hóa ổ cứng).
    *   Sạc tại trạm sạc công cộng bị hack (Kỹ thuật **Juice Jacking** - Trạm sạc sao chép dữ liệu qua dây cáp USB).
2.  **Ứng dụng (App Level):**
    *   **Insecure Data Storage (Lưu trữ không an toàn):** Lập trình viên lưu mật khẩu, Token trong file text rõ ràng (Cleartext) trên bộ nhớ điện thoại (VD thư mục `SharedPreferences`). Thiết bị bị Root thì Hacker đọc được.
    *   **Hardcoded Secrets:** Chôn chặt chuỗi API Key hoặc Mật khẩu kết nối DB vào thẳng mã nguồn. Hacker dùng Apktool dịch ngược là lấy được.
    *   **Repackaging (Đóng gói lại):** Hacker lên mạng tải 1 game nổi tiếng (VD: Flappy Bird), dịch ngược nó, nhét mã độc Trojan vào, đóng gói lại và đăng lên các chợ ứng dụng lậu. Nạn nhân tải về chơi thì bị điều khiển.
3.  **Kết nối mạng (Network Level):**
    *   Thiết bị kết nối Wi-Fi công cộng không an toàn -> Bị Sniffing / Man-in-the-Middle cướp Session.

## 7. PROTOCOLS/PORTS/SERVICES
*   **Android Architecture:** Kiến trúc xếp lớp. Lõi dưới cùng là **Linux Kernel** (quản lý phần cứng/bộ nhớ) -> Thư viện (Libraries) -> Android Runtime (ART/Dalvik) -> Application Framework -> Applications (App).
*   **iOS Architecture:** Lõi dưới cùng là **Core OS** (Dựa trên Darwin/Unix) -> Core Services -> Media -> Cocoa Touch (Giao diện UI). iOS kín hơn Android rất nhiều, mọi app phải qua quy trình kiểm duyệt (Code Signing) gắt gao của Apple.

## 8. IMPORTANT NUMBERS & FACTS
*   Để chạy được các công cụ phân tích động như Burp Suite (chặn gói tin HTTPS từ App), Pentester phải vượt qua cơ chế **SSL Pinning** của ứng dụng (App chỉ tin tưởng chứng chỉ SSL ghim cứng của nó, không tin chứng chỉ của Burp cài vào máy). Tool để qua mặt: Frida hoặc Xposed.
*   **Giải pháp phòng chống Juice Jacking:** Sử dụng thiết bị **USB Data Blocker** (Cục sạc chỉ cho phép dòng điện 5V chạy qua, chặn đứt 2 dây truyền dữ liệu).

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Rooting (Android) | Jailbreaking (iOS) |
| :--- | :--- | :--- |
| **Mục đích chính** | Giành quyền Admin (Root) để truy cập sâu vào lõi Linux, xóa app rác của nhà mạng. | Thoát khỏi "nhà tù" của Apple để tải các ứng dụng lậu, chỉnh sửa giao diện. |
| **Hậu quả** | Phá vỡ Sandbox. App độc hại có thể làm thịt toàn bộ hệ thống. | Thiết bị cực kỳ dễ nhiễm mã độc vì mất đi lớp khiên bảo vệ của App Store. Mất bảo hành. |

| Giải pháp Doanh nghiệp | Chức năng (Exam hay hỏi) |
| :--- | :--- |
| **MDM** (Mobile Device Management) | Quản trị thiết bị. Ép đặt chính sách mật khẩu 6 số, xóa từ xa (Remote Wipe), cấm cài app ngoài. |
| **MAM** (Mobile Application Management) | Chỉ quản trị Ứng Dụng làm việc, không can thiệp vào máy cá nhân. Phân chia rõ "Vùng Công Ty" và "Vùng Cá Nhân". |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Juice Jacking:** Đề bài: *"Nhân viên cắm sạc điện thoại tại sân bay, vô tình bị đánh cắp toàn bộ ảnh và danh bạ. Đây là kỹ thuật gì?"* -> Chọn **Juice Jacking**.
*   **Thiết kế bảo mật cốt lõi:** Hỏi: *"Cơ chế nào trên Android đảm bảo một ứng dụng không thể đọc được bộ nhớ của ứng dụng khác?"* -> Chọn **Sandboxing**.
*   **Quản trị Doanh nghiệp:** Đề bài: *"Để hỗ trợ chính sách BYOD (mang thiết bị cá nhân đi làm) an toàn, quản trị viên nên triển khai công nghệ nào để xóa dữ liệu công ty từ xa khi nhân viên mất máy?"* -> ĐÁP ÁN: **MDM (Mobile Device Management)**.
*   **Decompilation:** Hỏi: *"Công cụ nào dùng để dịch ngược một ứng dụng Android (.apk) để đọc các file XML và code?"* -> Chọn **Apktool** hoặc **MobSF**.

## 11. COMMON CONFUSIONS
*   **OWASP Web vs OWASP Mobile:** Đừng nhầm lẫn!
    *   Web Top 1 thường là **Injection**.
    *   Mobile Top 1 thường là **Improper Platform Usage** (Lạm dụng tính năng HĐH) hoặc **Insecure Data Storage** (Lưu trữ kém an toàn cục bộ trên máy).
*   **Rooting vs Sideloading:** Sideloading đơn thuần là tải file APK từ web (không qua Google Play) và cài đặt (chỉ cần bật tùy chọn "Unknown Sources"). Rooting là phá vỡ quyền hệ thống. Bạn có thể Sideloading mà KHÔNG CẦN Root.

## 12. REAL-WORLD CONTEXT
*   **Spyware (Phần mềm gián điệp) cấp quốc gia:** **Pegasus** (do NSO Group phát triển) là phần mềm gián điệp khét tiếng nhất thế giới. Nó khai thác các lỗ hổng Zero-click (không cần nạn nhân làm gì, chỉ cần nhận 1 tin nhắn iMessage) để Jailbreak ngầm điện thoại iOS và chiếm quyền kiểm soát Mic, Camera, tin nhắn mã hóa (WhatsApp/Signal) trước khi các tin nhắn đó bị mã hóa gửi đi.
*   **Fake Banking Apps:** Ở VN hiện tại, chiêu trò lừa đảo lớn nhất là mạo danh Thuế, Công an gửi link tải tệp APK giả mạo. Nạn nhân vô tình Sideloading tệp APK này, cấp quyền Trợ năng (Accessibility Service). Ứng dụng độc hại sẽ đọc trộm mã OTP ngân hàng gửi về và tự động chuyển tiền.

## 13. QUICK REVISION
1.  **Cơ chế phòng thủ ngăn chặn 1 ứng dụng đọc dữ liệu của ứng dụng khác trên Mobile gọi là gì?** -> Sandboxing (Hộp cát).
2.  **Đuôi file cài đặt gốc của hệ điều hành Android và iOS là gì?** -> APK (Android) / IPA (iOS).
3.  **Tấn công đánh cắp dữ liệu qua cổng sạc USB công cộng gọi là gì?** -> Juice Jacking.
4.  **Hành vi can thiệp vào iOS để vượt qua kiểm soát của Apple cài ứng dụng ngoài gọi là gì?** -> Jailbreaking.
5.  **Giải pháp bảo mật (phần mềm) mà doanh nghiệp dùng để quản lý, ép buộc chính sách bảo mật lên điện thoại nhân viên là gì?** -> MDM (Mobile Device Management).
6.  **Việc lập trình viên chèn cứng mật khẩu/API Key vào bên trong mã nguồn ứng dụng gọi là lỗ hổng gì?** -> Hardcoded Secrets / Hardcoded Passwords.

## 14. MEMORY HOOKS
*   **Juice Jacking:** Uống nước ép (Juice/Sạc pin) nhưng bị trúng độc (bị cướp dữ liệu).
*   **BYOD (Bring Your Own Device):** Nhớ thành "Bring Your Own Danger" (Mang thảm họa đến công ty) để nhớ rằng nó rất nguy hiểm và bắt buộc phải quản lý bằng MDM.
*   **Sandbox (Hộp cát):** Đứa trẻ nào chơi ở hộp cát nhà nấy, không được lấy đồ chơi (dữ liệu) của đứa khác. 
*   **Root / Jailbreak:** Đập vỡ hộp cát.
