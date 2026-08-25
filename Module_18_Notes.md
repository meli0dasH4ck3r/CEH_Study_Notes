# 📚 CEH v13 Study Notes - Module 18: IoT and OT Hacking

> **[📝 LỜI KHUYÊN TỪ GIẢNG VIÊN - ĐỌC KỸ TRƯỚC KHI BẮT ĐẦU]**
> *   "Hãy nhớ nguyên tắc lõi: **AI is an overlay, not a replacement.** Sự bùng nổ của thiết bị vạn vật kết nối (IoT) tạo ra nguồn cung Botnet khổng lồ cho các mạng lưới AI-Driven DDoS. Tuy nhiên, điểm yếu cốt lõi của thiết bị IoT vẫn nằm ở Mật khẩu mặc định và Thiếu bản vá."
> *   "Hãy **tự vẽ tay sơ đồ mindmap** phân biệt rạch ròi giữa IoT (Camera, Tủ lạnh thông minh) và OT/ICS (Hệ thống điều khiển công nghiệp, nhà máy điện). Chúng khác biệt hoàn toàn về độ ưu tiên (IoT ưu tiên Giá rẻ/Tính năng, OT ưu tiên Tính Sẵn sàng tuyệt đối)."

---

## 1. Module Overview (Dạy điều gì? Tại sao quan trọng?)
*   **Dạy điều gì:** Khám phá cấu trúc, kiến trúc truyền thông của IoT (Internet of Things) và OT (Operational Technology). Cách Hacker định vị (Shodan) và xâm nhập vào Camera an ninh, Xe thông minh, hoặc nguy hiểm hơn là Nhà máy điện, Hệ thống điều khiển nước.
*   **Tại sao quan trọng:** Thế giới đang bao phủ bởi thiết bị IoT (Hàng chục tỷ thiết bị). Hầu hết chúng được sản xuất rẻ tiền, bảo mật cực kém, là miếng mồi ngon cho Hacker tạo Botnet. Đáng sợ hơn, các cuộc tấn công vào mạng OT (Công nghiệp) có thể gây ra thảm họa vật lý (Mất điện toàn thành phố, tràn hóa chất) chứ không chỉ dừng lại ở mất dữ liệu.

## 2. Core Concepts (Definition, Meaning, Why it matters)
*   **IoT (Internet of Things):** Mạng lưới các thiết bị điện tử dân dụng (Camera, Loa thông minh, Máy lạnh, Xe hơi) được kết nối Internet.
*   **OT (Operational Technology):** Công nghệ vận hành. Nhóm công nghệ/phần cứng dùng để giám sát và điều khiển trực tiếp các thiết bị công nghiệp (Van nước, Turbin điện, Băng chuyền).
*   **ICS (Industrial Control Systems):** Hệ thống điều khiển công nghiệp (Nằm bên trong OT).
*   **SCADA (Supervisory Control and Data Acquisition):** Trạm kiểm soát giám sát và thu thập dữ liệu trung tâm của một nhà máy/hệ thống OT.
*   **PLC (Programmable Logic Controller):** Bộ điều khiển logic lập trình được. Là các hộp máy tính nhỏ bọc thép kết nối trực tiếp với máy móc (Van, Động cơ) để nhận lệnh từ SCADA.
*   **Rolling Code:** Thuật toán nhảy mã (dùng ở chìa khóa xe hơi, cửa cuốn). Mỗi lần bấm khóa, chìa khóa sinh ra 1 mã mới. Tránh bị Hacker thu âm và phát lại (Replay attack).

## 3. KEYWORDS 
| Mức độ | Từ khóa | Ý nghĩa cốt lõi |
| :--- | :--- | :--- |
| 🔴 **MUST MEMORIZE** | **Shodan** | Công cụ tìm kiếm (Google) dành cho Hacker. Chuyên rà quét các thiết bị IoT, Webcams hớ hênh trên toàn cầu. |
| 🔴 **MUST MEMORIZE** | **Mirai Botnet** | Mạng lưới Botnet nổi tiếng nhất lịch sử, cấu thành từ hàng triệu IP Camera và Router để tấn công DDoS. |
| 🔴 **MUST MEMORIZE** | **Stuxnet** | Mã độc đầu tiên trong lịch sử (2010) phá hoại thành công hệ thống làm giàu Uranium của Iran. Vũ khí không gian mạng kinh điển nhất nhắm vào SCADA. |
| 🟠 **HIGH PRIORITY** | **MQTT / CoAP** | Các giao thức truyền tải siêu nhẹ (Lightweight) sinh ra dành riêng cho thiết bị IoT (thay thế cho HTTP nặng nề). |
| 🟠 **HIGH PRIORITY** | **Modbus / DNP3** | Giao thức truyền thông công nghiệp (OT). Điểm yếu chết người: Không có mã hóa, không có xác thực (Cleartext). |
| 🟡 **SHOULD KNOW** | **Air-gap** | Nguyên lý bảo mật vật lý tốt nhất cho OT: Cách ly hoàn toàn (Không cắm dây Internet) vào hệ thống điện lưới. |

## 4. TOOLS (Tool, Purpose, Used for, Important point)
| Tool Name | Phân loại | Mục đích & Ứng dụng | Điểm lưu ý quan trọng (Exam) |
| :--- | :--- | :--- | :--- |
| **Shodan / Censys** | IoT Search Engine | Tìm các thiết bị IoT đang mở port (VD: Camera IP mở cổng RTSP 554 không pass). | Khởi đầu của mọi vụ hack IoT. |
| **Firmwalker** | Firmware Analyzer | Công cụ tự động quét mã nguồn (Firmware - tệp `.bin`) của bộ định tuyến, camera để tìm mật khẩu (Hardcoded pass), chứng chỉ ẩn. | Khai thác tĩnh tĩnh. |
| **HackRF / Flipper Zero** | SDR Hardware | Phần cứng quét sóng vô tuyến (Software Defined Radio). Bắt, ghi âm và phát lại tín hiệu mở cửa cuốn, mở xe hơi. | Tấn công vật lý. |
| **AI Threat Intelligence** | **AI-Powered** | Giải pháp bảo mật dùng AI để theo dõi toàn cảnh (Toàn cầu) các chủng mã độc IoT mới xuất hiện. | Blue Team phòng thủ. |

## 5. COMMANDS (Command, Purpose, Important options)
> [!NOTE]
> Module này thiên về kiến trúc và giao thức. Hãy nhớ các truy vấn tìm kiếm cơ bản của Shodan.

*   **Shodan Search Filters:**
    *   `port:554 has_screenshot:true`: Tìm các IP Camera mở port 554 và Shodan đã chụp sẵn màn hình hớ hênh.
    *   `org:"Tên công ty" product:"Apache"`: Tìm server Apache của công ty đó.
    *   `port:502`: Tìm các thiết bị công nghiệp chạy giao thức Modbus (cực kỳ nguy hiểm nếu mở ra Internet).

## 6. ATTACKS & TECHNIQUES
### IoT Attack Vectors:
1.  **Default/Weak Passwords:** Lỗ hổng lớn nhất của IoT. Nhà sản xuất thường để pass mặc định `admin/admin` hoặc `admin/123456` và người dùng không bao giờ đổi.
2.  **Firmware Extraction:** Hacker lên trang chủ hãng tải file Firmware nâng cấp (đuôi `.bin`), sau đó dùng lệnh `binwalk` để giải nén file đó, moi mật khẩu hoặc tạo Backdoor rồi build lại up lên máy nạn nhân.
3.  **Replay Attack (Tấn công phát lại):** Dành cho IoT cửa cuốn/xe hơi. Hacker dùng máy thu sóng, ghi lại tín hiệu bạn bấm mở cửa. Lần sau, hacker phát lại đoạn ghi âm đó để mở cửa. (Để chống lại, dùng Rolling Code).
4.  **Jamming (Gây nhiễu):** Dùng sóng vô tuyến công suất cao đè bẹp sóng Wi-Fi/Bluetooth của thiết bị IoT (camera chống trộm) khiến nó mù.

### OT/ICS Attack Vectors:
*   Mạng OT vốn dĩ là mạng Khép kín (Air-gapped) và Không an toàn (Giao thức Modbus không mã hóa). Ngày nay, doanh nghiệp cố gắng cắm OT vào chung với mạng CNTT (IT) để lãnh đạo dễ xem báo cáo. -> **IT/OT Convergence** (Sự hội tụ IT/OT).
*   **Đòn tấn công:** Hacker hack vào máy tính văn phòng (IT) bằng Phishing -> Từ mạng IT nhảy (Pivot) sang mạng OT (vì phân vùng mạng yếu kém) -> Gửi lệnh Modbus giả mạo để ra lệnh cho van nước mở tối đa, gây nổ áp suất.

## 7. PROTOCOLS/PORTS/SERVICES
*   **Giao thức IoT:** 
    *   **MQTT (Port 1883/8883):** Rất phổ biến cho nhà thông minh. (Bản chất giống Pub/Sub).
    *   **Zigbee / Z-Wave:** Giao thức sóng radio tiết kiệm điện cho nhà thông minh (khóa cửa, bóng đèn).
*   **Giao thức OT:**
    *   **Modbus (Port 502):** Giao thức công nghiệp kinh điển nhất. Nhược điểm: Truyền văn bản gốc (Cleartext), không có xác thực (No Authentication).

## 8. IMPORTANT NUMBERS & FACTS
*   **IoT Architecture (Kiến trúc 3 tầng):**
    1.  **Edge / Device Layer:** (Tầng thiết bị) Các cảm biến (Sensors), cơ cấu chấp hành (Actuators) ở biên.
    2.  **Gateway / Communication Layer:** (Tầng giao tiếp) Cổng trung chuyển dữ liệu lên Cloud.
    3.  **Cloud / Application Layer:** (Tầng đám mây) Nơi phân tích và lưu trữ.
*   **Tam giác CIA trong OT:** Khác với IT (Bảo mật dữ liệu - Confidentiality là số 1). Trong OT (Nhà máy), **Sẵn sàng (Availability)** và **An toàn sinh mạng (Safety)** luôn được đặt lên hàng đầu.

## 9. COMPARE & DIFFERENTIATE
| Tiêu chí | Môi trường IT (Công nghệ thông tin) | Môi trường OT (Công nghệ vận hành) |
| :--- | :--- | :--- |
| **Mục tiêu ưu tiên** | **C**IA (Bảo mật/Confidentiality lên đầu). | **A**IC (Sẵn sàng/Availability lên đầu). |
| **Hệ điều hành** | Windows, Linux, MacOS (Thường xuyên cập nhật bản vá). | Hệ thống nhúng thời gian thực (RTOS), Windows XP (Không bao giờ dám cập nhật vì sợ sập). |
| **Chu kỳ vòng đời** | 3-5 năm thay mới phần cứng. | 15-20 năm (Đòi hỏi sự ổn định tuyệt đối). |
| **Tác động nếu bị Hack** | Mất tiền, mất uy tín, lộ dữ liệu (Thiệt hại thông tin). | Tràn hóa chất, cháy nổ, chết người (Thiệt hại vật lý). |

## 10. CEH EXAM FOCUS (Format ôn thi tốt)
*   **Tìm kiếm IoT:** Đề bài: *"Công cụ tìm kiếm nào chuyên dùng để định vị các thiết bị IoT, Router, và SCADA bị phơi bày trên Internet?"* -> ĐÁP ÁN: **Shodan**.
*   **Mạng Botnet vĩ đại:** Câu hỏi về mạng lưới hàng triệu camera bị lợi dụng để DDoS -> ĐÁP ÁN: **Mirai Botnet** (Khai thác lỗ hổng mật khẩu mặc định qua Telnet).
*   **OT Attack kinh điển:** Câu hỏi về sâu máy tính đầu tiên tấn công phá hoại hạ tầng công nghiệp (Cụ thể là trạm làm giàu Uranium) -> ĐÁP ÁN: **Stuxnet**.
*   **Kiến trúc IoT:** Hỏi: *"Thành phần nào trong mô hình IoT làm nhiệm vụ thu thập dữ liệu vật lý (như nhiệt độ, ánh sáng) để gửi đi?"* -> Chọn **Sensors (Cảm biến)** ở tầng Edge.
*   **Giao thức OT:** *"Giao thức công nghiệp phổ biến (Port 502) có điểm yếu chết người là thiếu tính năng mã hóa và xác thực?"* -> **Modbus**.

## 11. COMMON CONFUSIONS
*   **Sensors vs Actuators:** Cả 2 đều ở lớp Edge.
    *   *Sensor (Cảm biến):* Bộ phận "Cảm nhận" (Nhìn, nghe, đo nhiệt độ). Thu thập đầu vào (Input).
    *   *Actuator (Bộ chấp hành):* Bộ phận "Ra tay" (Bật công tắc, khóa cửa, quay motor). Thực thi đầu ra (Output).
*   **Rolling Code vs Static Code:** Static code (Mã tĩnh) bấm phát nào mã phát đó giống hệt nhau -> Dễ bị Replay Attack. Rolling code (Mã cuộn/Mã nhảy) tự đổi sau mỗi lần bấm -> Chống Replay Attack.

## 12. REAL-WORLD CONTEXT
*   **Hacking Xe Jeep (2015):** Hai nhà nghiên cứu bảo mật đã trình diễn việc hack vào chiếc xe Jeep Cherokee đang chạy trên cao tốc. Bằng cách lợi dụng điểm yếu của hệ thống giải trí (Infotainment) có cắm SIM Internet (IoT), họ đã đột nhập thành công vào mạng CAN Bus (mạng lõi điều khiển xe - OT) và chiếm quyền phanh xe từ xa. Sự kiện này là ví dụ điển hình nhất về việc hội tụ IT/OT gây rủi ro chết người.

## 13. QUICK REVISION
1.  **Công cụ nào mệnh danh là "Google của Hacker" chuyên tìm thiết bị IoT?** -> Shodan.
2.  **Mã độc (Sâu) đầu tiên nhắm vào hệ thống công nghiệp SCADA/PLC là gì?** -> Stuxnet.
3.  **Tấn công lấy trộm sóng mở cửa xe hơi bằng máy ghi âm vô tuyến rồi mở lại gọi là gì?** -> Replay Attack.
4.  **Cơ chế mã hóa sinh ra một mã duy nhất mới sau mỗi lần bấm chìa khóa để chống phát lại gọi là gì?** -> Rolling Code.
5.  **Trong tam giác bảo mật, mạng OT/ICS ưu tiên yếu tố nào nhất?** -> Tính Sẵn sàng (Availability - A).
6.  **Giao thức nhắn tin nhẹ (Port 1883) chuyên dùng cho IoT là gì?** -> MQTT.

## 14. MEMORY HOOKS
*   **OT (Operational):** Công nghệ Mồ Hôi (Nhà máy, dầu khí, điện nước). Nếu sập là thảm họa vật lý. Ưu tiên Availability (Luôn phải chạy).
*   **IT (Information):** Công nghệ Máy Lạnh (Văn phòng, Server). Nếu sập là mất tiền/dữ liệu. Ưu tiên Confidentiality (Giữ bí mật).
*   **Mirai (Tiếng Nhật là Tương lai):** Một tương lai đáng sợ khi cái tủ lạnh và cái bóng đèn hùa nhau bắn sập website nhà nước (DDoS).
