# Escalation Ticket

## 1. Thông tin chung
- **Ticket ID:** ESC-D11-B2-01
- **Người tạo:** Role C (Diagnostic & Lead)
- **Giai đoạn:** Day 11 - Post-diagnostic / Rework
- **Mức độ nghiêm trọng:** High

## 2. Mô tả sự cố
- Phát hiện tỷ lệ sai lệch lớn giữa nhãn thủ công và nhãn tham chiếu (Reference B2): 21 lỗi SPURIOUS và 18 lỗi MISSING tập trung ở khu vực center zone.
- Xảy ra hiện tượng bất đồng thuận lớn giữa model predictions và annotator boxes ở ngưỡng IoU 0.50 và 0.70.

## 3. Đề xuất giải pháp & Phân công
- Đã ban hành bản vá guideline `20_guideline_patch.md` quy định rõ ràng về quy tắc che khuất và bounding box tight-fit.
- Yêu cầu Role B cập nhật lại findings và kiểm tra chất lượng trên toàn bộ các frame có độ nghi ngờ cao (như `adasind_019560.jpg`).
- Đồng thuận phê duyệt bộ nhãn rework làm cơ sở chuẩn để nghiệm thu.
