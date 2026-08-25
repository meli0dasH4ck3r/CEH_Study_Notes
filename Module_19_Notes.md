# 📚 CEH v13 Study Notes - Module 19: Cloud Computing

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Trong Cloud, AI (CSPM) giám sát hàng vạn cấu hình theo thời gian thực để tìm các quyền IAM bị cấp quá tay. Tuy nhiên, nguyên tắc xương sống của bảo mật đám mây là **Mô hình Trách nhiệm Chung (Shared Responsibility Model)**. Bạn đưa dữ liệu lên Cloud (AWS/Azure), nếu bạn cấu hình sai (mở Public Bucket) thì bạn mất dữ liệu, Cloud Provider không chịu trách nhiệm!"
> *   "Hãy **tự vẽ tay sơ đồ mindmap** về 3 mô hình dịch vụ (IaaS, PaaS, SaaS). Đề thi luôn hỏi ai là người chịu trách nhiệm cho HĐH, ai chịu trách nhiệm cho Dữ liệu trong từng mô hình."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Tìm hiểu các mô hình dịch vụ Đám mây (Cloud), kiến trúc bảo mật ảo hóa (Container, Docker, Kubernetes, Serverless). Các điểm mù bảo mật trong Đám mây (như rò rỉ thông tin từ S3 Bucket mở) và cách Hacker chiếm quyền IAM.
*   **Tại sao quan trọng:** Hiện tại 90% doanh nghiệp đã chuyển lên Đám mây. Nhưng Đám mây chỉ là "máy tính của người khác". Hacker không còn đục tường rào vật lý nữa, chúng đi lùng sục các thư mục lưu trữ (S3 Buckets) cấu hình quên cài mật khẩu, hoặc cướp Token của lập trình viên để truy cập toàn bộ tài nguyên công ty trên AWS/GCP.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **Cloud Computing:** Cung cấp tài nguyên máy tính (Server, Storage, Database) qua Internet. Xài bao nhiêu trả tiền bấy nhiêu.
*   **Shared Responsibility Model:** Mô hình trách nhiệm chung. Nhà cung cấp (AWS/Azure) bảo vệ "An ninh CỦA Đám mây" (Tòa nhà vật lý, mạng lõi, máy chủ vật lý). Khách hàng bảo vệ "An ninh TRONG Đám mây" (Dữ liệu, Ứng dụng, Quyền truy cập IAM).
*   **Virtualization (Ảo hóa):** Dùng phần mềm Hypervisor chia 1 máy chủ vật lý lớn thành nhiều máy chủ ảo (VM) nhỏ chạy HĐH độc lập.
*   **Containers (Docker):** Ảo hóa ở cấp độ Hệ điều hành. Các ứng dụng được đóng gói cùng thư viện của nó vào các thùng (Container). Container nhẹ hơn VM rất nhiều vì chúng dùng chung nhân (Kernel) của HĐH gốc.
*   **Kubernetes (K8s):** "Nhạc trưởng" điều phối hàng vạn Container.
*   **Serverless Computing:** Lập trình viên chỉ cần viết Code (Function), đẩy lên Cloud. Cloud tự chạy đoạn code đó khi có sự kiện (Event) mà không cần lập trình viên phải quan tâm đến việc tạo Server hay cài HĐH (VD: AWS Lambda).

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **IAM (Identity & Access Management)** | Bộ não phân quyền của Cloud. Lộ API Key hoặc IAM Token = Mất toàn bộ tài nguyên Cloud. |
| 🔴 **MUST MEMORIZE** | **IaaS, PaaS, SaaS** | 3 mô hình dịch vụ cốt lõi của Cloud (Infrastructure, Platform, Software). |
| 🔴 **MUST MEMORIZE** | **S3 Bucket / Blob Storage** | Kho lưu trữ dữ liệu dạng Object (Đối tượng) trên Cloud. Chỗ hay bị rò rỉ dữ liệu nhất. |
| 🟠 **HIGH PRIORITY** | **Container Escape** | Kỹ thuật Hacker thoát khỏi cái "Thùng" (Container) chật hẹp để đánh vào HĐH lõi (Host OS). |
| 🟠 **HIGH PRIORITY** | **CSPM** | (Cloud Security Posture Management) Giải pháp tự động rà quét lỗi cấu hình (Misconfiguration) trên Cloud. |
| 🟡 **SHOULD KNOW** | **VPC (Virtual Private Cloud)** | Mạng ảo nội bộ do khách hàng tự định nghĩa trên Cloud để cô lập tài nguyên của mình. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **CloudGoat / flaws.cloud** | Vulnerable Cloud | Môi trường AWS giả lập lỗi cố ý để luyện tập Cloud Pentesting. | Công cụ học tập đỉnh cao. |
| **ScoutSuite / Prowler** | Cloud Security Scanner | Quét tài khoản AWS/Azure/GCP để tìm các lỗ hổng cấu hình IAM, S3 đang mở Public. | Công cụ Blue Team cực mạnh. |
| **Pacu** | Cloud Exploitation | Framework tấn công AWS do Red Team sử dụng (Tương tự Metasploit nhưng dành cho Cloud). | Công cụ khai thác Cloud (Red Team). |
| **Trivy / Clair** | Container Security | Công cụ rà quét file ảnh (Docker Image) xem có chứa thư viện cũ hoặc chứa mã độc trước khi chạy hay không. | Dùng trong DevSecOps pipeline. |
| **AI CSPM** | **AI-Powered** | Hệ thống Quản trị Tư thế Bảo mật dùng AI để theo dõi toàn cảnh Cloud, tự động đóng các Bucket mở bậy. | Vũ khí phòng thủ v13. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!NOTE]
> Cloud Hacking xoay quanh việc lấy cắp API Key và dùng AWS CLI (Command Line Interface).

*   **AWS CLI Commands (Cơ bản):**
    *   `aws s3 ls`: Liệt kê các danh sách tài nguyên trong S3 Bucket (Nếu cấu hình lỗi `Allow All`, hacker gõ lệnh này sẽ xem được toàn bộ file).
    *   `aws sts get-caller-identity`: Lệnh cực kỳ quan trọng để Hacker xem mình đang nắm giữ "Quyền" (Role/Identity) của ai sau khi cướp được API Key. (Giống lệnh `whoami` trên Windows).

## 6. ATTACKS & TECHNIQUES
### Các Vector Tấn Công Đám Mây:
1.  **Misconfiguration (Cấu hình sai):** Đây là lỗi phổ biến chiếm 99% các vụ mất dữ liệu Cloud. Quản trị viên tải dữ liệu khách hàng lên Amazon S3, nhưng chọn nhầm quyền truy cập là "Public Read". Hacker chỉ cần gõ đúng link là tải được (Không cần hack).
2.  **IAM Credential Theft:** Hacker tìm thấy file `credentials` chứa API Key/Secret Key bị lập trình viên đưa nhầm lên GitHub. Từ đó Hacker dùng AWS CLI thao tác như chủ nhân.
3.  **Server-Side Request Forgery (SSRF):** Lỗ hổng kinh điển của Cloud. Hacker ép một Server AWS gửi request đến địa chỉ local ma thuật `169.254.169.254` (Trạm siêu dữ liệu - Metadata endpoint). Trạm này sẽ nhả ra API Key (Token) tạm thời của Server đó.
4.  **Container Escape (Vượt ngục Container):** Hacker dùng lỗ hổng ứng dụng (Web) để chạy mã độc vào Container. Sau đó lợi dụng việc Container chạy bằng quyền Root (Lỗi cấu hình), Hacker thoát ra ngoài, tấn công thẳng vào máy chủ Host chứa các Container khác.
5.  **Cryptojacking:** Hacker hack vào Cloud của bạn KHÔNG PHẢI để lấy dữ liệu. Chúng cướp tài khoản AWS của bạn, âm thầm tạo ra 100 máy chủ (EC2) cực mạnh để... Đào Tiền Ảo (Bitcoin). Cuối tháng bạn sẽ nhận hóa đơn tiền tỷ.

## 7. PROTOCOLS/PORTS/SERVICES
*   **Cổng API (REST APIs - HTTPS 443):** Mọi thao tác quản trị trên Cloud bản chất đều là gọi các hàm API. Bảo vệ Cloud chính là bảo vệ API.
*   **Docker API (Port 2375/2376):** Nếu cổng này bị phơi ra Internet mà không xác thực, Hacker có thể truy cập thẳng vào máy chủ Docker và chạy mã độc từ xa.

## 8. IMPORTANT NUMBERS & FACTS
*   **169.254.169.254:** Địa chỉ IP bất di bất dịch của dịch vụ **Instance Metadata (IMDS)** trên AWS. Đây là "chén thánh" của Cloud Hacker vì nó chứa thông tin cực kỳ nhạy cảm và Token tạm thời của máy chủ.

## 9. COMPARE & DIFFERENTIATE
> [!IMPORTANT]
> Câu hỏi về 3 Mô hình dịch vụ CHẮC CHẮN 100% CÓ TRONG ĐỀ THI.

| Mô hình | Nghĩa | Ai quản lý Máy Chủ (Server)? | Ai quản lý HĐH (OS)? | Ai quản lý Dữ liệu/App? | Ví dụ thực tế |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IaaS** | Hạ tầng (Nền móng) | Nhà cung cấp (AWS) | **Khách hàng tự cài HĐH** | Khách hàng | Mua Server ảo EC2 trên AWS. |
| **PaaS** | Nền tảng | Nhà cung cấp | Nhà cung cấp (Cài sẵn) | **Khách hàng chỉ viết Code** | AWS Elastic Beanstalk, Heroku. |
| **SaaS** | Phần mềm | Nhà cung cấp | Nhà cung cấp | Nhà cung cấp (Khách chỉ dùng) | Gmail, Google Drive, Office 365. |

| Tiêu chí | Virtual Machine (VM) | Container (Docker) |
| :--- | :--- | :--- |
| **Kiến trúc** | Chạy một HĐH riêng biệt (Guest OS) bên trong 1 HĐH chủ. Rất nặng nề. | Dùng chung HĐH chủ (Host OS kernel). Rất nhẹ, khởi động trong 1 giây. |
| **Bảo mật** | Cô lập rất tốt (Phần cứng). | Cô lập bằng phần mềm (Namespace/Cgroup). Dễ bị vượt ngục (Escape) hơn VM. |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Mô hình Cloud:** Đề hỏi: *"Mô hình dịch vụ đám mây nào cung cấp sẵn môi trường phát triển (Framework, OS) để lập trình viên chỉ việc mang Code lên chạy mà không cần quan tâm đến máy chủ?"* -> Đáp án: **PaaS (Platform as a Service)**.
*   **Lỗi SSRF lấy Token:** Đề hỏi: *"Hacker dùng lỗ hổng Web truy cập vào địa chỉ 169.254.169.254. Lỗ hổng nào đang được khai thác và đích đến là gì?"* -> Lỗ hổng **SSRF**, đích đến là **Instance Metadata Service (để lấy IAM Credentials)**.
*   **Đào tiền ảo:** Hỏi *"Sau khi hệ thống bị xâm nhập, CPU máy chủ liên tục chạy 100% không rõ lý do. Tấn công gì?"* -> **Cryptojacking**.
*   **Mô hình Trách nhiệm chung:** Đề bài *"Nếu dữ liệu khách hàng lưu trên AWS S3 bị lộ do cấu hình Public, ai là người chịu trách nhiệm về pháp lý theo mô hình Shared Responsibility?"* -> Đáp án: **Khách hàng (Customer)**.
*   **Ảo hóa (Virtualization):** Hỏi *"Công nghệ cốt lõi nào cho phép Cloud Computing phân chia tài nguyên máy tính vật lý thành các phiên bản ảo hóa?"* -> Chọn **Hypervisor**.

## 11. COMMON CONFUSIONS
*   **Container vs Serverless:** 
    *   *Container:* Bạn đóng gói ứng dụng lại mang lên Cloud. Ứng dụng vẫn chạy 24/7 (tốn tiền 24/7).
    *   *Serverless (Lambda):* Bạn chỉ mang đoạn Code lên. Khi có người bấm nút (Event), Cloud mới mượn tạm Server chạy đoạn Code đó mất 0.5 giây rồi tắt. Trả tiền 0.5 giây đó. Kiến trúc này khiến Hacker mất đi "nền tảng HĐH" để cắm mã độc lâu dài.
*   **SaaS vs PaaS:** SaaS là sản phẩm ĐÃ HOÀN THIỆN để End-user dùng (Gmail). PaaS là CÔNG CỤ XÂY DỰNG để Developer viết app của riêng họ.

## 12. REAL-WORLD CONTEXT
*   **Capital One Breach (2019):** Vụ rò rỉ dữ liệu thẻ tín dụng 100 triệu người của ngân hàng Capital One. Nguyên nhân hoàn toàn từ lỗi **SSRF**. Đám mây AWS của họ được bảo vệ rất chắc, nhưng 1 ứng dụng WAF mã nguồn mở cấu hình sai đã cho phép Hacker (Paige Thompson) gửi 1 lệnh SSRF lấy được Token của chính chiếc WAF đó từ siêu dữ liệu (Metadata). Sau đó cô ta dùng Token đó lấy toàn quyền đọc mọi bucket S3 của ngân hàng.
*   **Bảo vệ S3:** Ở các công ty hiện nay, bài toán đầu tiên của kỹ sư bảo mật là dùng công cụ **Prowler/ScoutSuite** quét hàng ngày xem có ông Dev nào vui tính lỡ đổi cài đặt Bucket từ Private sang Public hay không.

## 13. QUICK REVISION
1.  **Dịch vụ Office 365, Gmail thuộc mô hình Điện toán đám mây nào?** -> SaaS (Software as a Service).
2.  **Lỗ hổng nào chuyên dùng để lấy cắp IAM Token từ trạm siêu dữ liệu đám mây (169.254.169.254)?** -> SSRF (Server-Side Request Forgery).
3.  **Công cụ nguồn mở ảo hóa mức Hệ điều hành, giúp chạy các ứng dụng cực nhẹ và độc lập là gì?** -> Container (Docker).
4.  **Tình huống Hacker cướp tài khoản Cloud không để lấy dữ liệu mà để đào Bitcoin gọi là gì?** -> Cryptojacking.
5.  **Giải pháp tự động giám sát cấu hình bảo mật trên Cloud gọi là gì?** -> CSPM (Cloud Security Posture Management).
6.  **Theo Mô hình Trách nhiệm chung, ai chịu trách nhiệm bảo vệ hạ tầng máy chủ vật lý?** -> Nhà cung cấp Cloud (AWS/Azure).

## 14. MEMORY HOOKS
*   **IaaS (Pizza mua vỏ về tự nướng):** Hạ tầng nguyên thủy. Bạn tự lo lò nướng, phô mai.
*   **PaaS (Pizza giao tận nhà):** Có nền tảng sẵn. Bạn chỉ việc mở ra ăn (Viết code).
*   **SaaS (Ăn Pizza ở nhà hàng):** Người ta phục vụ từ A-Z. Bạn chỉ việc bỏ tiền ra xài (Gmail).
*   **Metadata (169.254.169.254):** Kho báu giấu dưới gầm giường của mọi máy chủ Cloud. Biết cách nhìn xuống gầm giường (SSRF) là thấy Token.
