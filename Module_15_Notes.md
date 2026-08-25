# 📚 CEH v13 Study Notes - Module 15: SQL Injection

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Hacker có thể dùng AI (như Sqlmap kết hợp AI Fuzzing) để tự động hóa việc tìm lỗ hổng SQLi, sinh ra hàng ngàn câu lệnh chèn phức tạp vượt WAF. Nhưng nếu bạn không hiểu cú pháp `UNION SELECT` hoặc mệnh đề `WHERE`, bạn không thể phân tích được lỗi."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân loại 3 dạng SQLi chính: In-band (Trực tiếp), Inferential/Blind (Mù), và Out-of-band (Ngoại luồng). Đây là xương sống của Module này."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Cách thức thao túng các ô nhập liệu (Input) của Website để chèn mã độc SQL (Structured Query Language) thẳng vào Cơ sở dữ liệu (Database) phía sau máy chủ.
*   **Tại sao quan trọng:** SQL Injection (SQLi) là một trong những lỗ hổng lâu đời nhất, tàn phá nhất và phổ biến nhất mọi thời đại. Nếu khai thác thành công, Hacker có thể vượt qua màn hình đăng nhập (không cần pass), đọc toàn bộ dữ liệu thẻ tín dụng, xóa sạch Database, hoặc thậm chí chiếm quyền điều khiển hệ điều hành máy chủ (OS Command Execution).

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **SQL Injection:** Lỗ hổng xảy ra khi dữ liệu người dùng nhập vào (Input) không được làm sạch (Sanitize) mà ghép thẳng vào câu lệnh SQL. Kết quả là ký tự nhập vào làm thay đổi logic ban đầu của câu lệnh.
*   **Tautology (Đồng hằng đúng):** Kỹ thuật tiêm biểu thức luôn ĐÚNG (ví dụ `1=1`) để lừa Database bỏ qua việc kiểm tra mật khẩu.
*   **Blind SQLi (SQLi mù):** Khi trang Web không hiện ra bảng dữ liệu hay thông báo lỗi SQL lên màn hình, Hacker phải đặt các câu hỏi "Đúng/Sai" (True/False) cho Database và quan sát thời gian phản hồi hoặc sự thay đổi nhỏ trên Web để suy ra dữ liệu.

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **' OR '1'='1** | Chuỗi payload Tautology kinh điển nhất mọi thời đại để Bypass Login. |
| 🔴 **MUST MEMORIZE** | **UNION SQLi** | Dùng toán tử `UNION` để gộp kết quả của câu lệnh độc hại vào câu lệnh hợp lệ để lấy dữ liệu. |
| 🔴 **MUST MEMORIZE** | **Blind SQLi** | Kỹ thuật hack khi Server KHÔNG trả về lỗi. Dùng thời gian (Time-based) hoặc suy luận (Boolean). |
| 🟠 **HIGH PRIORITY** | **xp_cmdshell** | Thủ tục lưu trữ (Stored Procedure) trên MS SQL Server, cho phép Hacker chạy lệnh HĐH (RCE). |
| 🟠 **HIGH PRIORITY** | **sqlmap** | Tool tự động hóa hack SQLi số 1 thế giới. |
| 🟡 **SHOULD KNOW** | **Prepared Statements** | Kỹ thuật lập trình (Parameterization) - **Cách phòng thủ tuyệt đối nhất** chống lại SQLi. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Sqlmap** | Automated SQLi Tool | Tự động phát hiện lỗ hổng, khai thác, lấy cắp toàn bộ DB, bẻ khóa Hash mật khẩu, và chiếm quyền điều hành (OS Shell). | Tool mạnh nhất và nổi tiếng nhất. Lệnh cơ bản phải thuộc. |
| **Burp Suite** | Web Proxy | Dùng để đánh chặn, sửa thông số POST/GET để chèn mã SQL. | Tool bắt buộc để test thủ công. |
| **Havij** | SQLi GUI Tool | Tool tự động dạng giao diện đồ họa (GUI) rất phổ biến ngày xưa. | Ít dùng thực tế nhưng hay gặp trong đề thi. |
| **SQLNinja** | SQLi Tool | Chuyên dùng để khai thác ứng dụng Web chạy trên **Microsoft SQL Server**. | Tập trung vào việc leo thang đặc quyền. |
| **AI SQL Fuzzers** | **AI-Powered** | Các tool thế hệ mới dùng LLM phân tích cấu trúc Web, tự sinh ra Payload SQLi biến đổi liên tục để qua mặt WAF. | CEHv13 nhấn mạnh sự nguy hiểm của AI Fuzzing. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!IMPORTANT]
> CEH exam cực kỳ thích hỏi về cú pháp của công cụ **sqlmap**. Bạn PHẢI nắm rõ thứ tự chạy.

*   **Sqlmap - Cú pháp lấy dữ liệu (Theo thứ tự từ to đến nhỏ):**
    1.  `sqlmap -u "http://site.com/id=1" --dbs`: Liệt kê tất cả danh sách Tên cơ sở dữ liệu.
    2.  `sqlmap -u "http://site.com/id=1" -D db_name --tables`: Liệt kê các Bảng (Tables) trong DB `db_name`.
    3.  `sqlmap -u "http://site.com/id=1" -D db_name -T users --columns`: Liệt kê các Cột (Columns) trong bảng `users`.
    4.  `sqlmap -u "http://site.com/id=1" -D db_name -T users -C username,password --dump`: Tải xuống (Dump) dữ liệu thật.
    5.  `sqlmap -u "http://site.com/id=1" --os-shell`: Cố gắng chiếm quyền thực thi lệnh trên Server (RCE).

*   **SQLi Payloads thủ công (Cơ bản):**
    *   `admin' --`: Đăng nhập với tên admin, phần còn lại (mật khẩu) bị biến thành chú thích (comment `--`) nên bỏ qua.
    *   `admin' #`: Tương tự `--` nhưng dùng cho MySQL.

## 6. ATTACKS & TECHNIQUES
### Phân loại các nhóm SQL Injection:
1.  **In-band SQLi (Cùng luồng / Trực tiếp):** Kẻ tấn công gửi Payload và nhận kết quả trả về ngay trên cùng một giao diện Web.
    *   **Error-based:** Ép Database phun ra thông báo lỗi (chứa phiên bản DB, cú pháp, cấu trúc bảng). Rất dễ hack.
    *   **UNION-based:** Dùng toán tử `UNION` để ghép câu lệnh độc hại vào câu hợp lệ. Yêu cầu số lượng cột (Columns) của câu lệnh độc hại phải BẰNG số cột của câu hợp lệ.
2.  **Inferential / Blind SQLi (Ngoại suy / Mù):** Web không hiện bảng hay lỗi gì cả.
    *   **Boolean-based (Dựa trên Đúng/Sai):** Hacker chèn câu hỏi (VD: "Chữ cái đầu tiên của mật khẩu Admin là 'a' đúng không?"). Nếu đúng, trang web load bình thường. Nếu sai, trang web load thiếu 1 đoạn text. Hacker dựa vào đó để đoán từng ký tự.
    *   **Time-based (Dựa trên thời gian):** Hacker dùng hàm `SLEEP(10)` hoặc `WAITFOR DELAY`. (VD: "Nếu mật khẩu là 'a', hãy ngủ 10 giây"). Nếu thấy Web xoay vòng vòng 10 giây mới load xong -> Đoán đúng chữ 'a'. Cực kỳ chậm nhưng chắc chắn.
3.  **Out-of-band SQLi (Ngoại luồng):** Hacker tiêm mã, nhưng Server không trả kết quả về Web, mà tự động gửi 1 gói tin DNS hoặc HTTP mang theo dữ liệu DB bắn về máy chủ của Hacker. Dùng khi mạng nội bộ chặn gắt gao.

## 7. PROTOCOLS/PORTS/SERVICES
SQL Injection chủ yếu tấn công thông qua giao thức **HTTP/HTTPS (Port 80/443)**. Tuy nhiên, mục tiêu cuối cùng là các hệ quản trị CSDL phía sau như:
*   **MySQL:** Port 3306. Lệnh comment: `#` hoặc `-- `
*   **Microsoft SQL Server (MSSQL):** Port 1433. Lệnh comment: `--`. Cực kỳ nguy hiểm với hàm `xp_cmdshell`.
*   **Oracle:** Port 1521.
*   **PostgreSQL:** Port 5432.

## 8. IMPORTANT NUMBERS & FACTS
*   **Bypass Authentication:** `SELECT * FROM Users WHERE Username='$user' AND Password='$password'`
    *   Nếu Hacker nhập vào ô Username: `' OR '1'='1`
    *   Câu lệnh thành: `WHERE Username='' OR '1'='1' AND Password='...'`
    *   Vì `'1'='1'` luôn ĐÚNG, hệ thống cho phép đăng nhập thẳng vào tài khoản đầu tiên trong bảng (thường là Admin) mà không cần pass.
*   **Phòng thủ tuyệt đối:** Phương pháp duy nhất có thể diệt tận gốc 100% lỗ hổng SQLi là sử dụng **Parameterized Queries (Truy vấn có tham số / Prepared Statements)**. Khi đó, máy chủ sẽ xử lý toàn bộ Input như là chuỗi văn bản thuần túy (Text), dù Hacker có gõ `OR 1=1`, DB cũng chỉ coi đó là chữ, không thực thi.

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Error-based SQLi | UNION-based SQLi | Blind SQLi |
| :--- | :--- | :--- | :--- |
| **Cách Web phản hồi** | Hiện chi tiết thông báo lỗi SQL (SQL Syntax error). | Hiện thẳng dữ liệu bị cắp lên bảng/văn bản trên Web. | Không hiện gì khác ngoài một trang bình thường hoặc trang trắng. |
| **Độ khó** | Dễ nhất. | Trung bình (cần tìm đúng số lượng cột). | Khó nhất, chậm nhất (phải đoán từng ký tự). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Cách Phòng Thủ:** Câu hỏi kinh điển: *"Biện pháp nào dưới đây là tốt nhất để phòng chống SQL Injection hoàn toàn?"* -> ĐÁP ÁN: **Parameterized Queries** hoặc **Prepared Statements**. (Input Validation, WAF hay Escaping characters chỉ là biện pháp hỗ trợ).
*   **Cú pháp Sqlmap:** Đề hỏi: *"Lệnh sqlmap nào dùng để trích xuất (download) dữ liệu mật khẩu?"* -> Chọn lệnh có đuôi `--dump`.
*   **Tautology:** Đề hỏi: *"Chuỗi `' OR '1'='1` thuộc kỹ thuật tấn công nào?"* -> Chọn **Tautology (Đồng hằng đúng)**.
*   **Time-based Blind:** Đề mô tả *"Hacker chèn chuỗi WAITFOR DELAY '0:0:10' và thấy trang web load mất 10 giây. Đây là tấn công gì?"* -> Chọn **Time-based Blind SQLi**.
*   **Đoán số lượng Cột (UNION):** Kỹ thuật dùng lệnh `ORDER BY 1`, `ORDER BY 2`... cho đến khi Web báo lỗi là để xác định bảng hợp lệ có bao nhiêu cột, phục vụ cho **UNION SQLi**.

## 11. COMMON CONFUSIONS
*   **SQLi vs XSS:** 
    *   *SQLi* đánh vào Cơ sở dữ liệu (Database) để trộm thông tin hệ thống. 
    *   *XSS* đánh vào Trình duyệt của nạn nhân (Browser) để trộm Cookie của nạn nhân.
*   **WAF (Web Application Firewall):** Dùng WAF chặn các chuỗi `SELECT`, `UNION` rất hiệu quả, nhưng Hacker có thể qua mặt bằng cách **Obfuscate** (Làm rối) thành `S e l e c t` hoặc dùng kỹ thuật Encoding. Vậy nên WAF không thể thay thế Prepared Statements.

## 12. REAL-WORLD CONTEXT
*   **WAF Bypass bằng AI:** Trong thực tế, các WAF hiện đại đều dùng AI chặn SQLi. Nhưng Hacker cũng viết script dùng AI (LLM) để sinh ra hàng vạn biến thể payload. Ví dụ, thay vì gửi `UNION SELECT`, AI sinh ra `/*!50000UNION*//**//*!50000SELECT*/`. WAF không hiểu cú pháp rác này nên cho qua, nhưng MySQL Server lại tự động bỏ đi các comment và thực thi nó.
*   **Hack Website mua sắm:** Các cuộc tấn công SQLi hiện nay rất hiếm ở các website lớn, nhưng đầy rẫy ở các website thương mại điện tử nhỏ, các trang bán hàng dựng bằng PHP thuần. Hacker thường nhắm thẳng vào bảng `Orders` hoặc `CreditCards`.

## 13. QUICK REVISION
1.  **Dạng SQLi nào sử dụng hàm SLEEP() hoặc WAITFOR DELAY để suy luận dữ liệu?** -> Time-based Blind SQLi.
2.  **Toán tử SQL nào dùng để gộp kết quả của 2 câu truy vấn lại với nhau?** -> UNION.
3.  **Công cụ tự động hóa khai thác SQLi phổ biến nhất thế giới là gì?** -> sqlmap.
4.  **Cờ (Option) nào trong sqlmap dùng để trích xuất thẳng dữ liệu trong DB ra file text?** -> `--dump`.
5.  **Biện pháp lập trình nào ngăn chặn 100% SQL Injection?** -> Prepared Statements / Parameterized Queries.
6.  **Chuỗi `' OR 1=1 --` thuộc loại kỹ thuật SQLi nào?** -> Tautology (Bypass đăng nhập).

## 14. MEMORY HOOKS
*   **Tautology:** Luôn luôn ĐÚNG. (1=1). Khóa nào cũng mở được.
*   **Blind SQLi:** Hack trong bóng tối. Trộm mò mẫm sờ từng đồ vật (đoán từng ký tự) thay vì nhìn thấy toàn bộ. Chậm nhưng vẫn lấy được đồ.
*   **Prepared Statements:** Cánh cửa thép phân loại rác. Mã độc (SQL command) bị coi là "rác" (văn bản) thay vì được coi là "mã lệnh".
