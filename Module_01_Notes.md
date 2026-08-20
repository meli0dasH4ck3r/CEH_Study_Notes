# CEH v13 Study Notes - Module 01: Introduction to Ethical Hacking

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
- **Nội dung cốt lõi:** Module này giới thiệu các khái niệm nền tảng về An toàn thông tin (Information Security), các loại hình tấn công mạng, các giai đoạn của một cuộc tấn công (Hacking Methodologies), các hình mẫu Hacker, vai trò của AI/ML trong an ninh mạng hiện đại, phân tích rủi ro, và các khung pháp lý/tiêu chuẩn bảo mật quốc tế.
- **Tại sao quan trọng:** Đây là "bệ phóng" cho toàn bộ chương trình CEH. Nắm vững những khái niệm cơ sở giúp người học định hình tư duy của một Ethical Hacker, thấu hiểu cách thức hoạt động của kẻ tấn công (thông qua Cyber Kill Chain, MITRE ATT&CK), từ đó biết cách xây dựng chiến lược phòng thủ đa lớp (Defense-in-Depth) hiệu quả và tuân thủ chặt chẽ pháp luật.
- **Scope & Limitations of Ethical Hacking (Phạm vi & Hạn chế):** Xác định rõ phạm vi được phép kiểm thử (Scope), tuyệt đối không vượt quá giới hạn hợp đồng. Hạn chế (Limitations): Không thể phát hiện 100% lỗ hổng, báo cáo chỉ mang tính chất thời điểm.
- **Skills of an Ethical Hacker:** Yêu cầu kỹ năng Kỹ thuật (Technical: OS, Network, Security concepts, Hacking tools) và Phi kỹ thuật (Non-technical: Báo cáo, Đạo đức nghề nghiệp, Kỹ năng giao tiếp).

## 2. Core Concepts (Khái niệm cốt lõi)
- **Information Security Elements (Các yếu tố cốt lõi của An toàn thông tin):**
  - **Confidentiality (Bảo mật):** Đảm bảo thông tin chỉ được truy cập bởi các cá nhân có thẩm quyền. (Ngăn chặn lộ lọt dữ liệu).
  - **Integrity (Tính toàn vẹn):** Đảm bảo thông tin không bị thay đổi hoặc giả mạo một cách trái phép trong quá trình lưu trữ hoặc truyền tải (Đảm bảo sự tin cậy và chính xác).
  - **Availability (Tính sẵn sàng):** Đảm bảo thông tin và hệ thống luôn hoạt động bình thường và sẵn sàng phục vụ khi người dùng hợp lệ có nhu cầu truy cập.
  - **Authenticity (Tính xác thực):** Đảm bảo nguồn gốc của thông tin là chính xác (Ví dụ: kiểm tra sinh trắc học, thẻ thông minh, mật khẩu).
  - **Non-Repudiation (Chống chối bỏ):** Đảm bảo người gửi hoặc người nhận không thể phủ nhận việc họ đã thực hiện một hành động hoặc giao dịch mạng (Ví dụ: Chữ ký số, Log files).
- **Information Assurance (IA - Đảm bảo thông tin):** Khái niệm bao trùm việc đảm bảo sự đáng tin cậy của thông tin thông qua 5 trụ cột: Integrity, Availability, Confidentiality, Authenticity, Non-repudiation.
- **Hacking Methodologies (Phương pháp luận tấn công):**
  - **Cyber Kill Chain:** Khung phân tích các giai đoạn của một cuộc tấn công do Lockheed Martin phát triển, giúp tổ chức nhận diện và phá vỡ chuỗi xâm nhập của kẻ tấn công.
  - **MITRE ATT&CK Framework:** Cơ sở dữ liệu và khung kiến thức toàn cầu về các chiến thuật (Tactics) và kỹ thuật (Techniques) thực tế của kẻ tấn công (Adversary TTPs).
  - **Diamond Model of Intrusion Analysis:** Mô hình phân tích xâm nhập có hình thoi gồm 4 phần cốt lõi: Kẻ thù (Adversary), Năng lực (Capability), Hạ tầng (Infrastructure) và Nạn nhân (Victim).
- **Threat Intelligence (Tình báo mối đe dọa - CTI):** Quá trình thu thập và phân tích thông tin về các mối đe dọa để chủ động phòng ngừa trước khi chúng xảy ra (Gồm 4 loại hình: Strategic, Tactical, Operational, Technical).
- **Risk Management (Quản lý rủi ro):** Quá trình liên tục gồm 4 bước: **Identification (Nhận diện) → Assessment (Đánh giá) → Treatment (Xử lý) → Tracking (Theo dõi)**. 
  - **Công thức lõi:** `Risk = Threat × Vulnerability` *(Chú thích: Đôi khi được mở rộng thành `Risk = Threat × Vulnerability × Impact` hoặc `Asset Value`)*.
  - Đi kèm với **Risk Matrix (Ma trận rủi ro)** để phân loại mức độ rủi ro dựa trên khả năng xảy ra (Likelihood) và tác động (Impact).
- **Incident Management (Quản lý sự cố):** Quy trình phản ứng, phân tích và khôi phục hệ thống mạng sau một sự cố bảo mật (IH&R - Incident Handling & Response).
- **Information Security Controls (Các biện pháp kiểm soát bảo mật):** Bao gồm Kiến trúc bảo mật doanh nghiệp (EISA - Enterprise Information Security Architecture) và Phân vùng mạng (Network Security Zoning).

## 3. KEYWORDS (Từ khóa quan trọng)
### 🔴 MUST MEMORIZE (Bắt buộc nhớ - Chắc chắn có trong bài thi)
- **Vulnerability:** Lỗ hổng, điểm yếu trong hệ thống, thiết kế, quy trình hoặc mã nguồn có thể bị lợi dụng.
- **Threat:** Mối đe dọa, một hoàn cảnh hoặc sự kiện tiềm ẩn (tin tặc, thiên tai, phần mềm độc hại) có thể gây thiệt hại cho hệ thống.
- **Exploit:** Mã khai thác, một đoạn mã hoặc chuỗi lệnh dùng để trục lợi từ một lỗ hổng bảo mật.
- **Payload:** Phần mã độc hại có chức năng phá hoại, thực thi hành động tấn công sau khi mã khai thác (exploit) đã mở đường thành công.
- **Zero-Day:** Lỗ hổng bảo mật hoàn toàn mới, chưa được nhà sản xuất phần mềm biết đến hoặc chưa có bản vá chính thức.
- **Defense-in-Depth:** Chiến lược phòng thủ chiều sâu, sử dụng nhiều lớp bảo vệ chồng chéo (Physical, Internal Network, Perimeter, Application, Data) để bảo vệ hệ thống.
- **Attacks Formula (Công thức tấn công):** `Attacks = Motive (Goal) + Method + Vulnerability`. Kẻ tấn công cần có Động cơ, Phương thức và Lỗ hổng để thực hiện thành công.
- **Types of Security Policies (4 loại chính sách bảo mật - Rất hay thi):**
  1. **Promiscuous (Dễ dãi):** Không có hạn chế, cực kỳ nguy hiểm.
  2. **Permissive (Cho phép rộng rãi):** Cho phép đa số, chỉ cấm các mối đe dọa đã biết.
  3. **Prudent (Thận trọng - Tốt nhất):** Chặn tất cả, chỉ cho phép những gì cần thiết (Nguyên tắc Default Deny).
  4. **Paranoid (Hoang tưởng):** Khắt khe nhất, chặn mọi thứ hoặc giới hạn cực đoan, làm giảm tính khả dụng.

### 🟠 HIGH PRIORITY (Ưu tiên cao)
- **Threat Sources (Nguồn mối đe dọa):** Phân loại thành 3 nhóm chính: **Network threats** (tấn công qua mạng), **Host threats** (tấn công nhắm vào máy chủ/hệ điều hành), **Application threats** (tấn công nhắm vào ứng dụng/phần mềm).
- **TTPs (Tactics, Techniques, and Procedures):** Các chiến thuật, kỹ thuật và quy trình tiêu chuẩn mô tả hành vi và phương thức hoạt động của kẻ thù.
- **OSINT (Open-Source Intelligence):** Thu thập tình báo từ các nguồn thông tin công cộng, mở trên mạng.
- **APT (Advanced Persistent Threat):** Mối đe dọa dai dẳng nâng cao, thường là các chiến dịch tấn công rất tinh vi, kéo dài, do các chính phủ tài trợ (State-Sponsored).
- **ISMS (Information Security Management System):** Hệ thống quản lý an toàn thông tin giúp đánh giá và kiểm soát rủi ro (ISO/IEC 27001).
- **IH&R (Incident Handling and Response):** Quy trình xử lý sự cố.

### 🟡 SHOULD KNOW (Nên biết - Phân loại Hacker)
- **Hacktivism:** Việc thực hiện các cuộc tấn công mạng với mục đích thúc đẩy các nghị trình chính trị, xã hội hoặc tôn giáo.
- **Script Kiddie:** Kẻ tấn công thiếu kỹ năng chuyên môn sâu, chỉ biết sử dụng các công cụ có sẵn.
- **Suicide Hacker:** Những tin tặc sẵn sàng tấn công phá hoại hạ tầng trọng yếu mà không hề quan tâm đến việc che giấu danh tính hay hình phạt có thể phải chịu.
- **Cyber Terrorist (Khủng bố mạng):** Kẻ tấn công với mục đích tạo ra sự sợ hãi, hoảng loạn hoặc gián đoạn quy mô lớn mang tầm vóc quốc gia.
- **Insider Threat:** Tấn công từ bên trong do nhân viên, nhà thầu lạm dụng quyền hạn.

## 4. TOOLS (Công cụ)
*[Lưu ý quan trọng: Module 01 CEH v13 đặc biệt nhấn mạnh vào AI Tools. Tuy nhiên, AI là phần MỚI được xếp chồng lên nền tảng InfoSec cốt lõi chứ KHÔNG thay thế các công cụ mạng truyền thống (sẽ học ở các module sau).]*
- **ChatGPT & OpenAI:** Có thể bị lợi dụng để tạo kịch bản tấn công, viết email lừa đảo mạo danh.
- **ShellGPT:** Trợ lý AI tích hợp trực tiếp vào terminal/command line, giúp tạo shell command, code snippet, tự động hóa OSINT.
- **AutoGPT:** Một AI agent tự trị (autonomous), có thể tự động hóa các chuỗi nhiệm vụ bảo mật phức tạp, tự thu thập thông tin và đưa ra quyết định mà không cần mồi lệnh liên tục.
- **WormGPT / FraudGPT / ChaosGPT:** Các biến thể LLM phi đạo đức (no ethical boundaries), được tối ưu hóa riêng cho giới tội phạm mạng để viết malware, email phishing, tạo trang web lừa đảo.
- **PentestGPT:** Trợ lý ngôn ngữ lớn được tinh chỉnh riêng để hỗ trợ các pentester phân tích lỗ hổng, vạch ra các hướng tấn công (attack vectors) tiếp theo.
- **BurpGPT / BugBountyGPT:** Tích hợp sức mạnh của AI vào quá trình Bug Bounty và phân tích luồng dữ liệu web (trong Burp Suite).

## 5. COMMANDS (Lệnh)
- **Jailbreak Prompts (Ví dụ: DAN - Do Anything Now):** Kỹ thuật prompt injection tinh vi yêu cầu AI vượt qua và phớt lờ các rào cản đạo đức được lập trình, ép AI sinh ra mã độc hoặc hướng dẫn làm điều cấm.
- **Prompt Injection:** Lừa mô hình ngôn ngữ lớn (LLM) bằng cách chèn các chỉ thị độc hại vào input để khiến nó hành xử ngoài dự định.

## 6. ATTACKS & TECHNIQUES (Các loại hình tấn công & kỹ thuật)
- **Passive Attacks (Tấn công thụ động):** Kẻ tấn công chặn bắt và giám sát luồng dữ liệu mà không làm thay đổi hệ thống. *Đặc điểm: Cực kỳ khó phát hiện. Mục đích: thu thập thông tin (Ví dụ: Sniffing, Eavesdropping).*
- **Active Attacks (Tấn công chủ động):** Kẻ tấn công thay đổi dữ liệu, hệ thống, hoặc mạng lưới. *Đặc điểm: Dễ phát hiện hơn nhưng hậu quả lớn (Ví dụ: DoS/DDoS, SQL Injection, Spoofing).*
- **Close-In Attacks (Tấn công cận cảnh):** Tiếp cận không gian vật lý vào hệ thống mạng hoặc thiết bị của nạn nhân (Ví dụ: Cắm USB mã độc, Shoulder Surfing).
- **Insider Attacks (Tấn công nội bộ):** Do những cá nhân có quyền truy cập hợp pháp cố ý hoặc vô ý làm rò rỉ dữ liệu, cấy backdoor.
- **Distribution Attacks (Tấn công phân phối - Supply Chain):** Kẻ tấn công can thiệp vào chuỗi cung ứng phần cứng hoặc phần mềm của sản phẩm trước khi nó đến tay người dùng cuối.
- **Top Attack Vectors hiện đại (v13 nhấn mạnh):** Ransomware (Mã độc tống tiền), IoT (Thiết bị kết nối mạng yếu kém), Cloud (Lỗ hổng cấu hình đám mây), Supply-chain, Botnet, Phishing.

## 7. PROTOCOLS / PORTS / SERVICES
- **EDI (Electronic Data Interchange):** Giao thức trao đổi dữ liệu điện tử y tế chuẩn hóa, bị ràng buộc nghiêm ngặt trong luật HIPAA.
- **POS (Point of Sale):** Các dịch vụ và thiết bị thanh toán thẻ, chịu sự điều chỉnh của tiêu chuẩn quốc tế PCI DSS.

## 8. IMPORTANT NUMBERS & FACTS (Các con số & Sự kiện quan trọng)
- **5 Giai đoạn CEH Hacking Methodology:** Reconnaissance ➔ Scanning ➔ Gaining Access ➔ Maintaining Access ➔ Clearing Tracks.
- **7 Giai đoạn Cyber Kill Chain:** Reconnaissance ➔ Weaponization ➔ Delivery ➔ Exploitation ➔ Installation ➔ C2 ➔ Actions on Objectives.
- **4 Lớp Diamond Model:** Adversary, Capability, Infrastructure, Victim.
- **4 Loại Threat Intelligence:** Strategic, Tactical, Operational, Technical.
- **4 Bước Risk Management:** Identification ➔ Assessment ➔ Treatment ➔ Tracking.
- **4 Loại Security Policy:** Promiscuous ➔ Permissive ➔ Prudent ➔ Paranoid.
- **9 Bước Incident Handling (IH&R):** Preparation, Recording, Triage, Notification, Containment, Evidence Gathering, Eradication, Recovery, Post-Incident Activities.

## 9. COMPARE & DIFFERENTIATE (So sánh và Phân biệt)

| Khái niệm 1 | Khái niệm 2 | Khái niệm 3 | Sự khác biệt cốt lõi |
|---|---|---|---|
| **Vulnerability** (Lỗ hổng) | **Threat** (Mối đe dọa) | **Risk** (Rủi ro) | - **Vulnerability**: Điểm yếu bản thân hệ thống. <br> - **Threat**: Yếu tố bên ngoài đe dọa hệ thống. <br> - **Risk**: Mức độ thiệt hại khi Threat khai thác thành công Vulnerability. |
| **White Hat** | **Black Hat** | **Gray Hat** | - **White Hat**: Hợp pháp, có xin phép. <br> - **Black Hat**: Phi pháp, phá hoại. <br> - **Gray Hat**: Không xin phép, nhưng không phá hoại (thường đòi tiền thưởng/Bug Bounty). |
| **Passive Attack** | **Active Attack** | N/A | - **Passive**: Nghe lén, không làm biến đổi dữ liệu, cực khó phát hiện. <br> - **Active**: Can thiệp làm sai lệch dữ liệu, gây gián đoạn, thường để lại log. |
| **Strategic CTI** | **Tactical CTI** | **Op/Tech CTI** | - **Strategic**: Cho C-level (CISO), tài chính. <br> - **Tactical**: Cho chuyên gia, tập trung vào TTPs. <br> - **Op/Tech**: Cho SOC/IR, tập trung vào IoCs (IP, hash). |
| **InfoSec** | **Cybersecurity** | **Information Assurance (IA)** | - **InfoSec**: Bảo vệ thông tin chung (kể cả giấy tờ). <br> - **Cybersecurity**: Bảo vệ hệ thống mạng và dữ liệu số. <br> - **IA**: Đảm bảo sự tin cậy thông tin (CIA + Authenticity + Non-repudiation). |
| **Permissive Policy** | **Prudent Policy** | N/A | - **Permissive**: Mở cửa cho mọi thứ, chỉ cấm kẻ thù đã biết (Default Allow). <br> - **Prudent**: Đóng cửa mọi thứ, chỉ cho phép những người được duyệt (Default Deny) - Phương pháp TỐT NHẤT. |

## 10. CEH EXAM FOCUS (Trọng tâm ôn thi CEH)
- 🔴 **Must Know:** 
  - Ghi nhớ chính xác **4 Policy Types** (Rất hay hỏi chọn Prudent là best practice).
  - Thuộc công thức **Attacks = Motive + Method + Vulnerability**.
  - Ghi nhớ thứ tự **5 giai đoạn Hacking (CEH)**, **7 giai đoạn Cyber Kill Chain** và **4 bước Risk Management**.
  - Phân biệt rõ: **Cyber Terrorist, Suicide Hacker, State-Sponsored Hacker**.
  - Công thức tính rủi ro lõi: **Risk = Threat x Vulnerability**.
  - Nắm vững lõi **CIA Triad**, tuyệt đối đừng nhầm lẫn giữa Integrity và Authenticity.
- 🟠 **High Priority:** 
  - Phân loại mối đe dọa theo nguồn (Network, Host, Application).
  - Hiểu mục đích của **MITRE ATT&CK Framework** (TTPs).
  - Phân biệt các đạo luật: **PCI DSS** (Thẻ tín dụng), **HIPAA** (Y tế), **SOX** (Tài chính kế toán), **GDPR** (Dữ liệu EU), **ISO/IEC 27001** (ISMS).
- 🟡 **Should Know:** 
  - Khái niệm **Defense-in-Depth** (Triết lý bảo vệ nhiều lớp).

## 11. COMMON CONFUSIONS (Những điểm dễ nhầm lẫn)
- **Vulnerability vs. Exploit vs. Payload:** 
  - Vulnerability là "ổ khóa bị lỏng" (điểm yếu). 
  - Exploit là "thanh sắt bẻ khóa" (công cụ lợi dụng điểm yếu). 
  - Payload là "quả bom" ném vào phòng sau khi mở cửa (mã độc thực thi cuối cùng).
- **Threat vs. Attack:** Threat là "nguy cơ có thể xảy ra", Attack là "hành động thực sự đang diễn ra".
- **Risk Assessment vs. Risk Management:** Assessment (Đánh giá) chỉ là một BƯỚC (số 2) trong quy trình Management (Quản lý) toàn diện gồm 4 bước.
- **Authenticity vs. Non-Repudiation:** Authenticity xác nhận "Bạn là ai" (VD: Login sinh trắc). Non-Repudiation đảm bảo "Bạn không thể chối việc đã làm" (VD: Chữ ký số giao dịch).

## 12. REAL-WORLD CONTEXT (Bối cảnh thực tế)
- **Sự trỗi dậy của AI Hacking:** Dùng **WormGPT** / **FraudGPT**, một kẻ thiếu kỹ năng (Script Kiddie) vẫn có thể gen ra malware qua mặt phần mềm diệt virus hoặc viết email Spear Phishing hoàn hảo. Tuy nhiên, kiến thức nền (TCP/IP, lỗ hổng) vẫn là cốt lõi.
- **Tuân thủ pháp luật (Compliance):** Lộ dữ liệu thẻ thanh toán do không dùng SSL ➔ vi phạm **PCI DSS**. Lộ dữ liệu công dân EU ➔ vi phạm **GDPR** (phạt tới 4% doanh thu toàn cầu).
- **State-Sponsored & APT:** Các nhóm hacker nhà nước dùng **Cyber Kill Chain** để giấu mình trong mạng (Supply-chain attack) suốt nhiều tháng nhằm gián điệp.

## 13. QUICK REVISION (Ôn tập siêu tốc)
- **CIA Triad** = Confidentiality, Integrity, Availability.
- **Hacking 5 steps:** Recon ➔ Scan ➔ Gain Access ➔ Maintain Access ➔ Clear Tracks.
- **Kill Chain 7 steps:** Recon ➔ Weaponization ➔ Delivery ➔ Exploitation ➔ Installation ➔ C2 ➔ Actions on Objectives.
- **Risk Management 4 steps:** Identify ➔ Assess ➔ Treat ➔ Track.
- **Risk Formula:** `Risk = Threat x Vulnerability` *(Chú thích: Có thể x Impact)*.
- **Attack Formula:** `Attack = Motive + Method + Vulnerability`.
- **4 Policies:** Promiscuous / Permissive / Prudent / Paranoid (Prudent là tốt nhất).
- **Threat Sources:** Network, Host, Application.
- **Cyber Terrorist:** Hacker đánh sập mạng lưới quốc gia gây sợ hãi.
- **Suicide Hacker:** Tấn công hủy diệt, không sợ đi tù.
- **Prudent Policy:** Default Deny (Cấm tất cả, chỉ mở cái cần).

## 14. MEMORY HOOKS (Mẹo ghi nhớ)
- **Vẽ mindmap các khái niệm có số:** Hãy nhớ tự vẽ ra giấy: 5 bước Hacking, 7 bước Kill Chain, 4 loại CTI, 4 loại Policy, 4 bước Quản lý Rủi ro.
- **Cyber Kill Chain (7 bước):** **"Rất Vui Được Khám Căn Hộ Anh"** ➔ **R**econ, **W**eaponization, **D**elivery, **E**xploitation, **I**nstallation, **C**2, **A**ctions.
- **Các loại Policy:** **4 chữ P** ➔ **P**romiscuous, **P**ermissive, **P**rudent, **P**aranoid. Trong đó **Prudent (Thận trọng)** là hoàn hảo nhất.
- **Phân loại Luật:**
  - Thẻ ngân hàng/Tiền ➔ **PCI DSS**
  - Bệnh án ➔ **HIPAA**
  - Kế toán/Gian lận chứng khoán ➔ **SOX**
  - Châu Âu/Cá nhân ➔ **GDPR**
- **Diamond Model (4 đỉnh):** **A-C-I-V** (Adversary, Capability, Infrastructure, Victim). *Kẻ ác (Adversary) dùng súng (Capability) nấp trong tòa nhà (Infrastructure) bắn nạn nhân (Victim).*
