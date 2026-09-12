# BÁO CÁO THIẾT KẾ PHÂN HỆ GIAO THUỐC CẤP CỨU HỎA TỐC RIKKEICARE EMERGENCY
**Khóa học:** Phân tích & Thiết kế Hệ thống (IT105)  
**Session:** 03 - Sáng tạo: Thiết kế toàn diện Phân hệ Giao thuốc Cấp cứu Hỏa tốc  
**Vai trò thực hiện:** Lead System Analyst (Lead SA)

---

## PHẦN 1: XÁC ĐỊNH STAKEHOLDER TRỌNG YẾU & PHÂN BỔ MA TRẬN

### 1. 3 Nhóm Tác nhân (Stakeholder) Trọng yếu
1. **Tài xế Cấp cứu Hỏa tốc (Emergency Courier):** Người tiếp cận trực tiếp hiện trường giao thuốc đúng mốc thời gian vàng.
2. **Điều координа viên Y tế Cấp cứu / Bác sĩ Trực ban (Emergency Medical Coordinator):** Nhóm chuyên môn theo dõi luồng cấp cứu, điều phối các phương án dự phòng khẩn cấp tại Trung tâm Điều hành.
3. **Cơ quan Cấp cứu Y tế 115 / Công an Phường địa phương (External Emergency Services):** Lực lượng chức năng hỗ trợ can thiệp mở cửa khẩn cấp khi xảy ra nguy cơ đe dọa tính mạng bệnh nhân.

---

### 2. Phân bổ Ma trận Quyền lực - Mức độ Quan tâm (Power - Interest Matrix)

| Nhóm chiến lược | Stakeholder | Lập luận Phân bổ vị trí |
| :--- | :--- | :--- |
| **Quản lý chặt chẽ** *(Manage Closely - Quyền lực Cao, Quan tâm Cao)* | **Điều phối viên Y tế Cấp cứu / Bác sĩ Trực ban** | Có quyền lực điều hành tuyệt đối (ra lệnh kích hoạt quy trình chuyển cấp, điều động xe 115) và mức độ quan tâm cao nhất đến tỷ lệ sống sót của bệnh nhân cũng như thời gian vàng 15 phút. |
| **Giữ thông tin** *(Keep Informed - Quyền lực Thấp, Quan tâm Cao)* | **Tài xế Cấp cứu Hỏa tốc** | Mức độ quan tâm rất cao vì trực tiếp đứng tại hiện trường chịu áp lực thời gian, nhưng quyền lực hạn chế (không tự ý phá cửa hay hủy đơn mà phải tuân theo chỉ thị từ hệ thống/Trung tâm điều hành). |
| **Giữ hài lòng** *(Keep Satisfied - Quyền lực Cao, Quan tâm Thấp)* | **Cơ quan Cấp cứu 115 / Công an Phường địa phương** | Có thẩm quyền pháp lý cao nhất trong việc can thiệp hiện trường (phá cửa/cấp cứu khẩn cấp), nhưng chỉ tham gia vào các tình huống báo động đỏ (mức độ quan tâm trực tiếp theo ca thấp cho đến khi được kích hoạt). |

---

## PHẦN 2: THIẾT KẾ CƠ CHẾ ỨNG PHÓ & YÊU CẦU HỆ THỐNG

### 1. Cơ chế Chuyển cấp (Escalation Process) & Ngắt thời gian chờ (Unreachable Timeout Trap)

Để xử lý bẫy mất liên lạc khẩn cấp mà không làm lãng phí "thời gian vàng" hay bỏ rơi bệnh nhân, hệ thống áp dụng cơ chế **Cảnh báo Báo động Đỏ 3 Cấp độ (Red-Alert Escalation)**:

1. **Phút thứ 12:00 - Kích hoạt Timeout Tự động (60 giây):** Khi tài xế đến vị trí và bấm nút "Đã đến hiện trường" trên app nhưng không có người mở cửa, hệ thống khởi chạy đếm ngược đúng **60 giây**. Đồng thời, hệ thống tự động kích hoạt tổng đài IVR gọi tự động với chuông báo động âm lượng tối đa liên tục 3 lần đến SĐT Bệnh nhân và 2 SĐT Thân nhân đã đăng ký khẩn cấp.
2. **Phút thứ 13:00 - Tự động Chuyển cấp (Auto-Escalation):** Nếu hết 60 giây ngắt kết nối mà không có phản hồi, hệ thống ngay lập tức đẩy cảnh báo Báo động Đỏ (Red Alert Popup) lên màn hình của **Điều phối viên Y tế tại Trung tâm**, đồng thời truyền dữ liệu vị trí GPS hiện tại, hình ảnh cửa nhà do tài xế chụp và mã bệnh án khẩn cấp.
3. **Phút thứ 13:30 - Kích hoạt Quy trình Ứng phó Song song:** 
   - Điều phối viên duyệt lệnh kích hoạt **Cuộc gọi liên thông khẩn cấp tới Tổng đài 115 và Công an Phường địa phương** gần nhất để yêu cầu can thiệp hiện trường.
   - Tài xế được hệ thống cấp phép đặt túi thuốc bảo quản vào **Hộp Thuốc Cấp Cứu Thông Minh (Smart Emergency Box)** gắn tại cửa hoặc giao cho Bảo vệ tòa nhà/Tổ trưởng dân phố (có chụp ảnh vị trí và quét mã QR xác nhận), giải phóng tài xế tiếp tục nhiệm vụ mới mà không làm đứt gãy luồng cứu hộ.

---

### 2. Đặc tả Yêu cầu FR & NFR

#### A. Yêu cầu Chức năng (Functional Requirement - FR)
- **Mã yêu cầu:** `FR-EMERG-01` (Cơ chế Chuyển cấp Báo động Đỏ Mất liên lạc Khẩn cấp)
- **Nội dung đặc tả:**  
  *"Khi tài xế bấm xác nhận 'Đã tới vị trí' nhưng không thể liên lạc với người nhận quá 60 giây, hệ thống phải tự động phát cuộc gọi IVR báo động âm lượng cao tới các số điện thoại khẩn cấp đã đăng ký; nếu tiếp tục không phản hồi sau 30 giây tiếp theo, hệ thống phải tự động kích hoạt cảnh báo Báo động Đỏ tới màn hình Điều phối viên Y tế, tự động gửi tọa độ GPS cùng dữ liệu bệnh án khẩn cấp tới Cơ quan 115 địa phương và cấp mã QR mở Hộp lưu trữ thuốc an toàn tại hiện trường cho tài xế."*

#### B. Yêu cầu Phi chức năng (Non-Functional Requirement - NFR)
- **Mã yêu cầu:** `NFR-EMERG-01` (Độ tin cậy vận hành & Thời gian phản hồi Cảnh báo)
- **Nội dung đặc tả:**  
  *"Phân hệ RikkeiCare Emergency phải đạt chỉ số độ tin cậy vận hành (Operational Reliability) đạt **99.999%** đối với luồng truyền nhận tin nhắn/cuộc gọi khẩn cấp, đảm bảo thời gian phát tín hiệu Báo động Đỏ từ ứng dụng tài xế về Trung tâm điều hành **$\le 1.0$ giây** và thời gian kết nối tổng đài cứu hộ khẩn cấp **$\le 3.0$ giây** trong mọi điều kiện tải hệ thống."*

---

### 3. Bộ User Story chuẩn 3 thành phần (Cho Tài xế Cấp cứu Hỏa tốc)

- **Mã User Story:** `US-EMERG-DRV-01`
- **Câu User Story:**  
  *Là một* **Tài xế Cấp cứu Hỏa tốc (Emergency Courier)**,  
  *Tôi muốn* **bấm nút kích hoạt 'Báo động Đỏ Mất liên lạc' ngay trên màn hình ứng dụng khi không thể gọi được người nhận sau 60 giây tại hiện trường**,  
  *Để* **hệ thống tự động chuyển giao trách nhiệm xử lý cứu hộ cho Trung tâm Điều phối Y tế và cấp phép gửi thuốc an toàn, giúp tôi giải phóng khỏi hiện trường đúng quy trình mà không bỏ rơi bệnh nhân hay làm lãng phí thời gian vàng của các ca cấp cứu khác.**
