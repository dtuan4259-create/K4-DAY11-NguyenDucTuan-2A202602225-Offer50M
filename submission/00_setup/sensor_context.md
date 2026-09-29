# Sensor context

— Rig: mô tả ngắn xe/camera gắn ở đâu theo hiểu biết của bạn từ ảnh (ADASIND không kèm tài liệu rig chi
tiết, ghi theo quan sát).

- Rig: Camera fisheye (mắt cá) góc siêu rộng được gắn ở vị trí phía trước/gương chiếu hậu của xe quan sát toàn cảnh (SVM/360 surround view) để ghi nhận không gian bãi đỗ xe và mặt đường.
  — `ego_body` nhìn thấy ở đâu trong frame (góc capo, gương, tay lái...).
  - `ego_body`: Nhìn thấy một phần nắp capo/thân xe hoặc cản trước ở mép dưới của khung hình.
    — Vòng kính (lens circle) nằm ở vị trí nào trong ảnh, chiếm khoảng bao nhiêu phần khung hình.
    - Vòng kính (lens circle): Vùng quan sát của ống kính mắt cá bao phủ gần như toàn bộ khung hình (khoảng 80-90% diện tích ảnh), có hiệu ứng méo quang học dạng cong (distortion) ở các mép viền.
