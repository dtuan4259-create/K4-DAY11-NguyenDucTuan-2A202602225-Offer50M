# Guideline Patch (Bản vá hướng dẫn gán nhãn)

## 1. Vấn đề phát hiện
- Tỷ lệ lỗi SPURIOUS và MISSING cao đột biến ở vùng trung tâm (center zone) trên block B2.
- Nhầm lẫn giữa bóng râm/nền đường với phương tiện, và bỏ sót các phương tiện bị che khuất một phần (occluded).

## 2. Quy tắc bổ sung / Sửa đổi
- **Quy tắc che khuất (Occlusion):** Chỉ gán nhãn phương tiện khi nhìn thấy ít nhất 20% thân xe. Bỏ qua các đối tượng bị che khuất trên 80% nếu không đủ đặc trưng nhận dạng rõ ràng.
- **Ranh giới Bounding Box (Tight-fit):** Hộp bao phải ôm sát mép ngoài cùng của đối tượng, không bao gồm bóng đổ (shadow) của phương tiện xuống mặt đường để tránh lỗi SPURIOUS.
- **Ngưỡng kích thước tối thiểu:** Bỏ qua các vật thể nhỏ hơn $15 \times 15$ pixel ở hậu cảnh xa trừ khi có yêu cầu đặc thù.

## 3. Kế hoạch triển khai
- **Người thực hiện:** Role B và Role C.
- **Phạm vi áp dụng:** Áp dụng ngay cho toàn bộ các slice còn lại và đợt review Rework.
