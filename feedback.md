# BÁO CÁO ĐÁNH GIÁ CÁC NHÓM DỰ ÁN AI

---

## Nhóm A4: AI Agent dự đoán bệnh và đặt lịch hẹn
* **Chủ đề:** Dự đoán bệnh và hỗ trợ đặt lịch khám.

| Tiêu chí đánh giá | Điểm số |
| :--- | :---: |
| Problem-solution fit | 4 |
| AI product thinking | 4 |
| Demo quality | 3 |

* **Điều làm tốt:** Metric đánh giá AI rất rõ ràng, có nhiều metric đánh giá trên các khía cạnh khác nhau để cải thiện như tỉ lệ đặt lịch, sensitivity, độ chính xác phân luồng bệnh nhân, latency, đánh giá mức độ hài lòng của người dùng.
* **Gợi ý cải thiện:** AI chưa tự chủ động hỏi thêm thông tin về triệu chứng của người bệnh.
    * **Giải pháp:** Xây dựng AI tự chủ động hỏi thêm tình hình bệnh nhân (Clarification Dialogue) cho đến khi loại bỏ các điểm mơ hồ và tìm ra căn bệnh gần sát nhất với các triệu chứng của bệnh nhân.

---

## Nhóm A5: AI hỗ trợ giải đáp và đặt lịch khám bác sĩ
* **Chủ đề:** Giải đáp y tế và tối ưu quy trình đặt lịch.

| Tiêu chí đánh giá | Điểm số |
| :--- | :---: |
| Problem-solution fit | 4 |
| AI product thinking | 4 |
| Demo quality | 3 |

* **Điều làm tốt:** Có Plan rõ ràng. Điểm tốt là team chỉ giới hạn ở phần dự đoán đơn giản bệnh, không phụ thuộc vào đấy để tự động đặt lịch, giúp việc đặt lịch khám không bị phụ thuộc vào cảm giác của bệnh nhân gây lãng phí tiền bạc.
* **Gợi ý cải thiện:** AI chưa xử lý tốt ở phần người dùng đặt lịch thử nghiệm hoặc đặt lịch nhưng bận không đi khám được.
    * **Giải pháp:** Đề xuất xây dựng hệ thống xác nhận liên tục hoặc gọi điện thoại cho người bệnh nếu người dùng khai báo gặp triệu chứng nguy cấp.

---

## Nhóm B5: Hệ thống phân loại chỉ số sức khỏe

| Tiêu chí đánh giá | Điểm số |
| :--- | :---: |
| Problem-solution fit | 4 |
| AI product thinking | 3 |
| Demo quality | 3 |

* **Điều làm tốt:** Có phân loại được các ngưỡng chỉ số một cách khoa học.
* **Gợi ý cải thiện:** Các ngưỡng chỉ số hiện đang bị cố định theo chuẩn quốc tế, chưa tối ưu cho dữ liệu liên quan đến Việt Nam.
    * **Giải pháp:** Thu thập thêm các chỉ số và dữ liệu tại Việt Nam để xét ngưỡng, thay thế dần các chỉ số cố định bằng dữ liệu thực tế tại địa phương.

---

## Nhóm B6: Chatbot trả lời theo Database

| Tiêu chí đánh giá | Điểm số |
| :--- | :---: |
| Problem-solution fit | 4 |
| AI product thinking | 3 |
| Demo quality | 4 |

* **Điều làm tốt:** Nhóm làm demo tốt, thể hiện được các tính năng để giải quyết bài toán. AI chatbot trả lời đúng theo dữ liệu có trong database, không bị ảo giác (hallucination) và không trả lời ngoài phạm vi (out of domain).
* **Gợi ý cải thiện:** Không có metric đánh giá AI chatbot nên không có căn cứ đánh giá và feedback để cải thiện AI.
    * **Giải pháp:** Đề xuất ghi thêm log hội thoại để thu thập dữ liệu, từ đó xây dựng các bộ metric đánh giá hiệu quả của chatbot.
