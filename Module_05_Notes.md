# 📚 CEH v13 Study Notes - Module 05: Vulnerability Analysis

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Nguyên lý lõi: **AI is an overlay, not a replacement.** CEH v13 giới thiệu hàng loạt công cụ AI như Pentest Copilot, SmartScanner... Nhưng nếu bạn không hiểu **CVSS Score** hay không phân biệt được **False Positive** và **False Negative**, bản báo cáo của AI chỉ là giấy lộn."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** về **Vulnerability Management Lifecycle (6 bước)**. Đề thi chắc chắn sẽ hỏi thứ tự các bước này."
> *   **Công thức tư duy:** `Vulnerability Assessment = Quét + Đối chiếu cơ sở dữ liệu (CVE)`. Nó CHƯA phải là Penetration Testing (vì chưa có bước khai thác - Exploit).

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Module này hướng dẫn cách đánh giá, phát hiện và định lượng các lỗ hổng bảo mật (Vulnerabilities) trong hệ thống của tổ chức (OS, Web, Network, Database). 
*   **Tại sao quan trọng:** Quét lỗ hổng tự động giúp tiết kiệm hàng ngàn giờ làm việc thủ công. Nó giúp trả lời câu hỏi: *"Trong hàng ngàn máy chủ, máy nào đang dính lỗi chưa vá (Missing patches), máy nào cấu hình sai (Misconfigurations)?"* từ đó Blue Team (Phòng thủ) có thể vá lỗi, hoặc Red Team (Tấn công) có thể chọn vũ khí (Exploit) phù hợp nhất.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Vulnerability (Lỗ hổng):** Là điểm yếu trong hệ thống, thiết kế, hoặc ứng dụng mà kẻ tấn công có thể lợi dụng.
*   **Vulnerability Management Lifecycle:** Quy trình vòng đời quản lý lỗ hổng (6 bước): Khởi tạo (Baseline) -> Quét (Assessment) -> Đánh giá rủi ro (Risk Assessment) -> Khắc phục (Remediation) -> Xác minh lại (Verification) -> Giám sát (Monitor).
*   **CVE (Common Vulnerabilities and Exposures):** Danh sách chuẩn hóa các lỗ hổng đã được công bố toàn cầu. Mỗi lỗ hổng có 1 mã định danh (VD: CVE-2021-44228 là lỗi Log4j).
*   **CVSS (Common Vulnerability Scoring System):** Hệ thống chấm điểm mức độ nghiêm trọng của lỗ hổng (từ 0.0 đến 10.0).
*   **Credentialed vs Non-Credentialed Scan:**
    *   **Credentialed:** Quét CÓ tài khoản (Scanner được cấp user/pass để login vào máy). Quét sâu, chính xác, phát hiện lỗi phần mềm, registry, thiếu patch.
    *   **Non-Credentialed:** Quét KHÔNG tài khoản (Đứng từ ngoài mạng nhìn vào). Chỉ thấy các lỗi hổng của cổng đang mở.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **CVSS** | Điểm số quyết định mức độ nghiêm trọng của lỗ hổng. |
| 🔴 **MUST MEMORIZE** | **CVE / NVD** | Cơ sở dữ liệu lỗ hổng bảo mật quốc gia (National Vulnerability Database). |
| 🔴 **MUST MEMORIZE** | **False Positive** | (Dương tính giả). Công cụ báo có lỗi, nhưng thực tế là không có. Khá phiền phức. |
| 🔴 **MUST MEMORIZE** | **False Negative** | (Âm tính giả). Máy có lỗi, nhưng công cụ báo an toàn. **Cực kỳ nguy hiểm!** |
| 🟠 **HIGH PRIORITY** | **Baseline** | Mức chuẩn bảo mật tối thiểu mà mọi máy tính trong công ty phải đạt được. |
| 🟠 **HIGH PRIORITY** | **Executive Report** | Báo cáo tóm tắt dành cho sếp (Cấp quản lý, CEO, CIO). Không chứa code, chỉ chứa rủi ro. |
| 🟡 **SHOULD KNOW** | **Technical Report** | Báo cáo kỹ thuật dành cho IT/Admin. Chứa mã CVE, POC (Proof of Concept) và cách vá lỗi. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)

> [!TIP]
> **[AI Overlay trong CEH v13]** Traditional Tools (công cụ truyền thống) thường dựa trên rule (luật) có sẵn, dễ bỏ sót mã độc mới. **AI-Powered Assessment Tools** trong v13 (như Bugbase Copilot) sử dụng LLM để tự động phân tích mã nguồn, phát hiện logic flaw (lỗi logic) mà các công cụ tĩnh không nhìn thấy, đồng thời tự động viết Báo cáo kỹ thuật.

| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Nessus Essentials** | Network Scanner | Công cụ đánh giá lỗ hổng phổ biến nhất thế giới (của Tenable). | Rất hay được hỏi trong đề thi. |
| **OpenVAS** | Network Scanner | Phiên bản mã nguồn mở, miễn phí của Nessus (Greenbone Networks). | Framework quét lỗ hổng toàn diện, free. |
| **Nikto** | Web Scanner | Quét các máy chủ Web (Web Servers) để tìm thư mục ẩn, cấu hình sai, CGI lỗi. | Chuyên dành cho Web Server. Rất ồn ào. |
| **InsightVM / Qualys** | Enterprise Scanner | Các giải pháp quét lỗ hổng quy mô lớn cho doanh nghiệp. | Tích hợp sâu với hạ tầng mạng. |
| **Skipfish** | Web Scanner | Công cụ quét ứng dụng web chủ động, viết bằng C, tốc độ cực nhanh. | Do Google phát triển. |
| **Pentest Copilot / Hackules** | **AI-Powered** | Trợ lý ảo AI giúp phân tích kết quả quét phức tạp và đưa ra gợi ý khai thác. | Công cụ mới của CEH v13. |
| **SmartScanner / Equixly** | **AI-Powered** | Tự động hóa quá trình tìm kiếm lỗ hổng bằng thuật toán Machine Learning. | Khắc phục điểm yếu của scan tĩnh. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> Dù Assessment thường dùng GUI (Nessus), CEH vẫn yêu cầu bạn biết cách dùng command line cơ bản.

*   **Nmap (NSE - Nmap Scripting Engine):**
    *   `nmap --script vuln <IP>`: Sử dụng danh sách script `vuln` mặc định của Nmap để rà quét các lỗ hổng đã biết trên mục tiêu. Rất hiệu quả và nhanh.
*   **Nikto (Web Vulnerability Scanner):**
    *   `nikto -h http://<IP>`: Quét máy chủ web tại địa chỉ IP đó.
    *   `nikto -h <IP> -Tuning 123`: Tùy chỉnh mức độ quét theo các Module cụ thể.

## 6. ATTACKS & TECHNIQUES (Quy trình phân tích lỗ hổng)

*   **Phân loại Công cụ Quét (Types of VA Tools):**
    *   **Host-based:** Cài thẳng vào máy chủ (như phần mềm diệt virus) để quét registry, file hệ thống, quyền user.
    *   **Application-layer:** Chuyên quét ứng dụng web (SQLi, XSS) hoặc database.
    *   **Scope assessment:** Quét diện rộng toàn mạng (Network-wide).
    *   **Active vs Passive:**
        *   **Active Tools:** Chủ động gửi payload hỏng để xem server sập hay phản hồi lỗi. (Rủi ro làm sập mạng).
        *   **Passive Tools:** Chỉ bắt gói tin (Sniffing) và so sánh phiên bản phần mềm (VD: Thấy gói tin chứa chữ `Apache 1.3` -> kết luận dính lỗ hổng cũ).
*   **Tree-based Assessment (Đánh giá dạng cây):** Quá trình quét đi từ tổng quát đến chi tiết. Đầu tiên tìm OS -> Sau đó tìm Service -> Sau đó tìm Lỗ hổng của Service đó.

## 7. PROTOCOLS/PORTS/SERVICES
Vulnerability Scanners không bị giới hạn ở giao thức nào. Chúng sẽ liên tục gửi các gói tin TCP/UDP đến **tất cả các cổng mở** được phát hiện trong pha Scanning (Module 03).
Tuy nhiên, quét **Credentialed Scan (Quét có tài khoản)** thường dựa vào 2 cổng:
*   **Port 22 (SSH):** Để scanner login vào máy chủ Linux.
*   **Port 139/445 (SMB) & 3389 (RDP):** Để scanner login vào máy chủ Windows.

## 8. IMPORTANT NUMBERS & FACTS
> [!WARNING]
> Bắt buộc thuộc bảng điểm **CVSS v3.x / v4.0** để đi thi.

| Điểm CVSS | Mức độ Nghiêm trọng (Severity) | Hành động cần thiết |
| :--- | :--- | :--- |
| **0.0** | None (Không có) | Thông tin thuần túy. |
| **0.1 - 3.9** | **Low (Thấp)** | Có thể vá khi có thời gian rảnh. |
| **4.0 - 6.9** | **Medium (Trung bình)** | Lên kế hoạch vá trong kỳ bảo trì tiếp theo. |
| **7.0 - 8.9** | **High (Cao)** | Yêu cầu vá gấp (Ví dụ: Lỗi rò rỉ dữ liệu). |
| **9.0 - 10.0** | **Critical (Nghiêm trọng)** | **Tắt máy, ngắt mạng, vá ngay lập tức.** (VD: RCE - Remote Code Execution). |

## 9. COMPARE & DIFFERENTIATE

### ⚖️ Vulnerability Assessment vs Penetration Testing
| Tiêu chí | Vulnerability Assessment (VA) | Penetration Testing (PT) |
| :--- | :--- | :--- |
| **Mục đích chính** | **Tìm và Liệt kê** càng nhiều lỗ hổng càng tốt. | Chứng minh lỗ hổng đó **có thể bị khai thác** thực tế. |
| **Hành động** | Chỉ Quét (Scan) và báo cáo. | Quét -> **Tấn công (Exploit)** -> Leo thang đặc quyền. |
| **Độ sâu** | Rộng nhưng cạn. | Hẹp nhưng sâu. |

### ⚖️ Executive Report vs Technical Report
> [!IMPORTANT]
> Đây là câu hỏi kinh điển của CEH. Luôn nhớ đối tượng người đọc.

*   **Executive Report (Báo cáo Quản trị):** Dành cho sếp, giám đốc. Chỉ chứa: Biểu đồ, Rủi ro kinh doanh, Chi phí tổn thất ước tính, Số lượng lỗ hổng High/Critical. **Không chứa mã code hay kỹ thuật.**
*   **Technical Report (Báo cáo Kỹ thuật):** Dành cho Admin/Developer. Chứa: Tên lỗ hổng (CVE), IP bị dính, Cổng nào, Gói tin Proof-of-concept, Cách nâng cấp/vá lỗi.

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **False Positive / False Negative:** Đề bài: *"Nhân viên IT chạy Nessus và Nessus báo máy chủ Web an toàn tuyệt đối. Tuy nhiên hôm sau máy chủ bị hack qua một lỗ hổng SQLi mới. Nessus đã mắc lỗi gì?"* -> Đáp án: **False Negative** (Báo âm tính giả).
*   **Các bước của VA Lifecycle:** Đề yêu cầu sắp xếp thứ tự: **Baseline -> Assessment -> Risk Assessment -> Remediation -> Verification -> Monitor**. (Ghi nhớ: Phải có Chuẩn (Baseline) trước mới có cái để so sánh. Khắc phục (Remediate) xong phải Xác minh (Verify) lại xem hết lỗi chưa).
*   **Credentialed Scan:** Đề hỏi: *"Loại scan nào ít tạo ra luồng traffic mạng nhất và cung cấp kết quả chi tiết nhất về các patch bị thiếu trên Windows?"* -> Đáp án: **Credentialed / Authenticated Scan**.
*   **CVSS:** Đề hỏi: *"Lỗ hổng có điểm 9.5 thuộc mức độ nào?"* -> Đáp án: **Critical**.

## 11. COMMON CONFUSIONS
*   **CVE vs CVSS:** CVE giống như **Biển số xe** (Định danh lỗ hổng). CVSS giống như **Vận tốc** của xe đó (Mức độ nguy hiểm của lỗ hổng).
*   **Nessus vs Nmap:** Nmap là công cụ Scanning (nhìn bên ngoài), Nessus là công cụ Vulnerability Assessment (khám sức khỏe toàn diện bên trong). (Dù Nmap có NSE vuln script, nhưng nó không quy mô và có báo cáo đẹp như Nessus).

## 12. REAL-WORLD CONTEXT
*   **Nguy cơ sập hệ thống (Denial of Service):** Trong thực tế doanh nghiệp, KHÔNG BAO GIỜ được chạy Nessus/OpenVAS quét hệ thống SCADA/ICS (hệ thống nhà máy) hoặc máy chủ legacy bằng các plugin "Dangerous" (Active Scan). Máy chủ cũ gặp gói tin dị thường sẽ bị tràn bộ đệm (Buffer Overflow) và treo (Blue Screen) ngay lập tức. Luôn quét vào giờ thấp điểm hoặc môi trường Staging.
*   **Rác (False Positives):** Các tool scan tĩnh thường trả về hàng ngàn trang False Positives. Công việc của một Security Analyst là dùng AI (như Pentest Copilot) hoặc kinh nghiệm để lọc ra 10 lỗi thực sự nguy hiểm từ 1000 lỗi đó.

## 13. QUICK REVISION
1.  **Công cụ nào được xem là chuẩn mực để quét lỗ hổng (Vulnerability Assessment)?** -> Nessus / OpenVAS.
2.  **Sự khác biệt giữa False Positive và False Negative?** -> False Positive báo lỗi sai (có báo không). False Negative báo an toàn sai (không báo có - nguy hiểm nhất).
3.  **Báo cáo nào chứa mã CVE và hướng dẫn vá lỗi?** -> Technical Report.
4.  **Kiểu quét nào có tài khoản đăng nhập để kiểm tra Registry và Patch?** -> Credentialed Scan.
5.  **Điểm CVSS v3 từ 7.0 - 8.9 thuộc phân loại nào?** -> High.
6.  **Công cụ AI mới nào hỗ trợ Pentester viết báo cáo lỗ hổng tự động trong CEHv13?** -> Pentest Copilot / Bugbase.

## 14. MEMORY HOOKS
*   **False Negative:** **N**egative = **N**o warning, but you're hacked! (Không cảnh báo, nhưng vẫn toang).
*   **VA Lifecycle:** **B**ig **A**pples **R**eally **R**equire **V**ery **M**uch care (Baseline, Assessment, Risk Assessment, Remediation, Verification, Monitor).
*   **CVSS Range:**
    *   3 - 6 - 8 - 10 (Mốc nhớ nhanh: Dưới 4 là Low, 4-7 là Med, 7-9 là High, 9-10 là Critical).
