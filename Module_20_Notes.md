# 📚 CEH v13 Study Notes - Module 20: Cryptography

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Trong Module cuối cùng này, AI đang mở ra một kỷ nguyên đen tối bằng cách kết hợp với **Máy tính Lượng tử (Quantum Computing)** đe dọa bẻ gãy mọi thuật toán mã hóa hiện tại (RSA). Nhưng nền tảng của mật mã học (Symmetric vs Asymmetric) vẫn là cái khung bạn bắt buộc phải nằm lòng."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân biệt 3 nhánh chính của Mật mã: Symmetric (Khóa Đối xứng), Asymmetric (Khóa Bất đối xứng), và Hashing (Băm). Module này thi lý thuyết rất nặng, không có lệnh console nào cả!"

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Tìm hiểu các khái niệm, thuật toán toán học đằng sau việc mã hóa (bảo vệ bí mật) thông tin và Hashing (Đảm bảo tính toàn vẹn). Ngoài ra còn tìm hiểu PKI (Hạ tầng khóa công khai) - hệ thống tạo ra chứng chỉ số HTTPS.
*   **Tại sao quan trọng:** Mật mã học là chốt chặn cuối cùng bảo vệ dữ liệu. Nếu Hacker vượt qua Firewall, vượt qua tài khoản Admin, nhưng dữ liệu trong ổ cứng bị mã hóa bởi chuẩn AES-256, thì dữ liệu đó cũng vô giá trị với Hacker.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Cryptography (Mật mã học):** Khoa học về việc xáo trộn văn bản rõ (Plaintext) thành văn bản mã hóa (Ciphertext) thông qua một "Chìa khóa" (Key).
*   **Symmetric Encryption (Khóa đối xứng):** Dùng CÙNG MỘT chìa khóa để khóa (Mã hóa) và mở (Giải mã). (Tốc độ Cực Nhanh).
*   **Asymmetric Encryption (Khóa bất đối xứng):** Dùng 2 chìa khóa CẶP với nhau: 1 Khóa Công khai (Public Key - Ai cũng xem được) dùng để mã hóa, 1 Khóa Bí mật (Private Key - Giữ kín) dùng để giải mã. (Tốc độ Rất Chậm).
*   **Hashing (Băm):** Thuật toán một chiều (Chỉ có chiều vào, không có chiều dịch ngược). Biến một đoạn văn bản (1 trang hay 1000 trang) thành một chuỗi ký tự có độ dài cố định. (VD: Sinh mã MD5, SHA-256). Dùng để kiểm tra "Tính toàn vẹn" (File có bị sửa không) và lưu Mật khẩu.
*   **Digital Signature (Chữ ký số):** Xác nhận người gửi là ai và văn bản không bị sửa. (Dùng Hashing + Khóa bí mật của người gửi).
*   **PKI (Public Key Infrastructure):** Hạ tầng chứng thực chữ ký số (HTTPS ổ khóa xanh), bao gồm tổ chức cấp phát chứng chỉ CA.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **AES (Advanced Encryption Standard)** | Thuật toán mã hóa Khóa Đối Xứng mạnh nhất và chuẩn nhất thế giới hiện nay. |
| 🔴 **MUST MEMORIZE** | **RSA** | Thuật toán mã hóa Khóa Bất Đối Xứng kinh điển nhất (Dựa trên số nguyên tố). |
| 🔴 **MUST MEMORIZE** | **SHA (Secure Hash Algorithm)** | Chuẩn thuật toán Băm an toàn (SHA-1, SHA-2, SHA-3). |
| 🟠 **HIGH PRIORITY** | **Salting** | Kỹ thuật thêm chuỗi ngẫu nhiên vào dữ liệu (Mật khẩu) trước khi đem Băm để chống tấn công Rainbow Table. |
| 🟠 **HIGH PRIORITY** | **CA (Certificate Authority)** | Cơ quan uy tín bên thứ ba đứng ra cấp phát và chứng nhận khóa Public Key. |
| 🟡 **SHOULD KNOW** | **Diffie-Hellman** | Thuật toán kinh điển giúp 2 người xa lạ trao đổi Khóa bí mật an toàn qua mạng hở. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Hashcalc / MD5Calculator** | Hash Generator | Tool sinh mã băm từ 1 file bất kỳ để kiểm tra xem file tải về có nguyên vẹn không. | Công cụ cơ bản nhất. |
| **VeraCrypt** | Disk Encryption | Tool mã hóa toàn bộ ổ cứng hoặc tạo ra vùng chứa ẩn (Hidden Volume). | Rất nổi tiếng sau khi TrueCrypt dừng phát triển. |
| **GPG / PGP** | Email Encryption | Mật mã khóa công khai dùng để mã hóa Email. Rất nổi tiếng trong giới hacker/whistleblower. | Bắt buộc cài khi muốn liên lạc an toàn. |
| **AI Quantum Crypto** | **AI-Powered** | Mối đe dọa v13: Máy tính lượng tử (Shor's algorithm) kết hợp AI sẽ phá vỡ hoàn toàn RSA trong thập kỷ tới. | Yêu cầu phải chuyển sang mã hóa Hậu Lượng Tử (Post-Quantum Crypto). |

## 5. COMMANDS (Command, Purpose, Important options)
> [!NOTE]
> Không có lệnh Console cụ thể trong đề thi cho module này. Thay vào đó, đề sẽ hỏi về kích thước Bit của các thuật toán. (Ví dụ AES có 3 bản: 128, 192, 256 bits).

## 6. ATTACKS & TECHNIQUES
### Các cuộc tấn công vào Mật Mã (Cryptography Attacks):
1.  **Brute-Force Attack:** Thử MỌI khả năng chìa khóa (Key) có thể có cho đến khi ra kết quả. Nếu thuật toán xài khóa 256 bit -> Chịu thua.
2.  **Known-Plaintext Attack:** Hacker đã nắm được (hoặc đoán được) một phần văn bản gốc (Plaintext) và đối chiếu nó với văn bản mã hóa (Ciphertext) để dịch ngược ra Chìa Khóa. (Giống kiểu giải trò chơi ô chữ).
3.  **Chosen-Plaintext Attack:** Hacker lừa được máy tính của nạn nhân tiến hành mã hóa một dòng văn bản do Hacker tự nghĩ ra, rồi xem nó mã hóa thành cái gì để bắt bài thuật toán.
4.  **Dictionary / Rainbow Table Attack:** (Dùng cho Băm). Xây dựng trước một cái bảng chứa 1 tỷ mật khẩu kèm theo mã Hash tương ứng của nó. Khi chộp được mã Hash, chỉ việc đem dò trong bảng (Rất nhanh).
5.  **Birthday Attack:** Tấn công "Nghịch lý ngày sinh". Dựa vào xác suất toán học để tìm ra 2 văn bản khác nhau nhưng lại Băm ra cùng một chuỗi Hash (gọi là **Collision - Đụng độ**).
6.  **Man-in-the-Middle (MiTM) Attack:** Kẻ tấn công đứng giữa 2 bên, chặn quá trình trao đổi Public Key ban đầu, và thay bằng Public Key của mình. (Giải pháp chống là dùng PKI/Certificate).

## 7. PROTOCOLS/PORTS/SERVICES
*   **IPsec:** Nằm ở Lớp Mạng (Network Layer - Layer 3). Mã hóa gói tin IP.
*   **TLS/SSL:** Nằm ở Lớp Giao vận/Ứng dụng. Mã hóa luồng dữ liệu (Web, Email). TLS (v1.2, v1.3) đã thay thế hoàn toàn chuẩn SSL cũ rích và nhiều lỗ hổng (SSL v3).
*   **SSH (Port 22):** Dùng RSA để tạo kênh liên lạc bảo mật.

## 8. IMPORTANT NUMBERS & FACTS
*   **Kích thước (Bit size):** Khóa càng dài (256, 512, 1024, 2048, 4096 bit) thì càng an toàn (Mã hóa mạnh) nhưng máy tính xử lý càng chậm.
*   **Mã hóa Ổ cứng:** BitLocker (Windows), FileVault (Mac).
*   **Mã hóa Cơ sở dữ liệu:** Không bao giờ lưu mật khẩu người dùng dưới dạng văn bản (Plaintext) hay mã hóa 2 chiều (Symmetric). **BẮT BUỘC phải Băm (Hashing) và Rắc muối (Salting)**. (Ví dụ: `Hash(Password + Salt)`.

## 9. COMPARE & DIFFERENTIATE
> [!IMPORTANT]
> Câu hỏi bẫy kinh điển: So sánh Đối Xứng và Bất Đối Xứng.

| Tiêu chí | Mã hóa Đối xứng (Symmetric) | Mã hóa Bất đối xứng (Asymmetric) |
| :--- | :--- | :--- |
| **Số lượng khóa (Key)** | 1 Khóa (Dùng chung cho cả Khóa & Mở). | 2 Khóa (Cặp Public Key - Private Key). |
| **Tốc độ mã hóa** | **Cực kỳ nhanh** (Dùng cho dữ liệu lớn/Ổ cứng). | Rất chậm (Dùng cho văn bản nhỏ). |
| **Ví dụ thuật toán** | **AES**, DES, 3DES, Blowfish. | **RSA**, ECC, Diffie-Hellman, DSA. |
| **Rủi ro lớn nhất** | Key Distribution: Làm sao để gửi chìa khóa cho người kia mà không bị nghe lén? | MiTM chặn đứng việc trao đổi Public Key ban đầu. |

**Bức tranh hoàn hảo (Hybrid):** Trong thực tế (như trang Web HTTPS), máy tính dùng Khóa Bất Đối Xứng (RSA - Chậm) để gói kín và trao đổi một cái "Chìa khóa Đối Xứng" (AES - Nhanh). Sau đó 2 bên trò chuyện mượt mà bằng chìa AES đó. (Kết hợp điểm mạnh của cả hai).

| Tiêu chí | Encryption (Mã hóa) | Hashing (Băm) |
| :--- | :--- | :--- |
| **Đảo ngược** | **Được** (Giải mã). | **Không** (Hàm một chiều). |
| **Mục tiêu** | Đảm bảo Tính Bí mật (Confidentiality). | Đảm bảo Tính Toàn vẹn (Integrity) / File không bị sửa. |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Xác định loại Thuật toán:** Đề cho 1 list `RSA, ECC, AES, DSA` và hỏi: *"Cái nào thuộc nhóm Symmetric?"* -> Đáp án là **AES**. Nếu hỏi *"Cái nào dùng phương trình toán học đường cong Elliptic để tiết kiệm pin trên thiết bị di động?"* -> Chọn **ECC (Elliptic Curve Cryptography)**.
*   **Salting:** Đề hỏi: *"Để chống lại các cuộc tấn công Rainbow Table vào mật khẩu băm, quản trị viên nên dùng tính năng gì?"* -> Chọn **Salting (Rắc muối)**.
*   **Chữ ký số (Digital Signature):** Đề hỏi quy trình ký: *"Người gửi dùng cái gì để tạo chữ ký số?"* -> ĐÁP ÁN: Dùng **Private Key CỦA CHÍNH NGƯỜI GỬI** (Để chứng minh "Đúng là tao gửi nè").
*   **Đụng độ (Collision):** Hỏi: *"Tấn công Birthday attack nhắm vào việc tìm ra lỗ hổng gì của hàm Băm?"* -> Chọn **Collision** (Tìm ra 2 văn bản khác nhau nhưng có chung mã Hash).
*   **Diffie-Hellman:** Câu hỏi: *"Thuật toán nào cho phép 2 thực thể trao đổi Key an toàn trên đường truyền không bảo mật?"* -> Chọn **Diffie-Hellman**.

## 11. COMMON CONFUSIONS
*   **Mã hóa (Mã hóa Bí Mật) vs Ký Số (Mã hóa Xác Thực):**
    *   *Mã hóa bí mật:* Tôi lấy **Public Key của BẠN** để khóa thư lại. (Chỉ mình bạn có Private Key để mở). Dùng để giấu thông tin.
    *   *Chữ ký số:* Tôi lấy **Private Key của TÔI** để đóng mộc (ký). (Ai có Public Key của tôi đều xác nhận được mộc này là hàng real). Dùng để chống chối bỏ.
*   **Encoding (Base64) vs Encryption:** Base64 KHÔNG PHẢI là mã hóa bảo mật. Base64 là Đổi Định Dạng (Encoding). Không có mật khẩu, ai cũng dịch ngược Base64 được. Đề thi hỏi Base64 để chống Hack là SAI.

## 12. REAL-WORLD CONTEXT
*   **Ransomware (Tống tiền):** Các băng đảng Ransomware cực kỳ thông minh. Chúng dùng HĐH Windows sinh ra một chìa khóa Đối Xứng (AES) cực nhanh để mã hóa ổ cứng của nạn nhân trong 10 phút. Sau đó, nó dùng Public Key của Hacker (RSA) để khóa cái chìa AES đó lại. Cuối cùng nó phá hủy chìa AES gốc. Cách duy nhất để nạn nhân cứu dữ liệu là phải mua cái "Private Key" của Hacker để giải mã, lấy lại chìa AES.
*   **Bảo mật Hậu Lượng Tử (PQC):** Máy tính lượng tử (Quantum Computer) của Google/IBM đang phát triển cực nhanh. Khi nó hoàn thiện, nó có thể dùng thuật toán Shor's giải ngược khóa RSA 2048-bit trong vài giây (thứ mà máy tính thường cần hàng triệu năm). Mỹ (NIST) hiện đã ban hành các tiêu chuẩn mã hóa mới để đối phó thảm họa tương lai này.

## 13. QUICK REVISION
1.  **Thuật toán mã hóa Khóa Đối xứng (Symmetric) phổ biến và mạnh nhất hiện nay là gì?** -> AES (Advanced Encryption Standard).
2.  **Khái niệm chỉ việc thêm chuỗi ký tự ngẫu nhiên vào mật khẩu trước khi Băm để làm nhiễu Hacker là gì?** -> Salting.
3.  **Tổ chức trung gian thứ ba đứng ra cấp phát chứng chỉ số HTTPS (PKI) gọi là gì?** -> CA (Certificate Authority).
4.  **Hai văn bản khác nhau nhưng khi chạy qua hàm SHA-1 lại sinh ra cùng một mã Băm (Hash) gọi là hiện tượng gì?** -> Collision (Đụng độ).
5.  **Thuật toán mã hóa Khóa Bất Đối Xứng (Asymmetric) kinh điển nhất dựa trên bài toán phân tích thừa số nguyên tố là gì?** -> RSA.
6.  **Chữ ký số (Digital Signature) sử dụng khóa nào của người gửi để mã hóa mã Băm?** -> Khóa bí mật (Private Key) của người gửi.

## 14. MEMORY HOOKS
*   **Đối xứng (Symmetric):** Chìa khóa nhà. Cầm chìa (khóa) để chốt cửa trước khi đi làm, rồi cũng cầm đúng chìa đó mở lúc về.
*   **Bất đối xứng (Asymmetric):** Ổ khóa bấm (Public Key). Phát ổ khóa cho 100 người bạn. Bạn nào có thư mật cứ bấm ổ khóa lại gửi cho tôi. Nhưng chỉ CÓ MÌNH TÔI cầm Chìa (Private Key) mở được đống ổ khóa đó.
*   **Birthday Attack (Nghịch lý ngày sinh):** Trong 1 phòng 23 người, có tới 50% cơ hội có 2 người trùng ngày sinh. Áp dụng vào Hashing: Rất dễ tìm ra 2 văn bản rác trùng mã Hash.
*   **Hash (Băm thịt):** Băm miếng thịt heo thành thịt băm (Mã hóa một chiều). Đố ai lấy thịt băm ráp lại thành miếng thịt heo ban đầu được. (File gốc không thể bị dịch ngược).
