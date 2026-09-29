# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Lát cắt 1: Block B2 - Zone center (frame tiêu biểu: `adasind_019560.jpg`) | 12 SPURIOUS, 11 MISSING (tổng lỗi cao nhất hệ thống) | Mật độ lỗi nghiêm trọng nhất tập trung tại vùng trung tâm quan sát, ảnh hưởng trực tiếp đến an toàn ADAS (bỏ sót vật thể hoặc nhận diện vật cản ảo trên làn di chuyển chính). | Ảnh crop bbox từ frame `adasind_019560.jpg` trong thư mục `screenshots/`, dòng log tương ứng trong `findings.csv` và kết quả sweep IoU tại threshold 0.50/0.70. |
| Lát cắt 2: Block B2 - Zone mid (frame tiêu biểu: `adasind_062370.jpg`) | 6 SPURIOUS, 4 MISSING, 1 ATTRIBUTE | Khu vực làn biên và vùng chuyển tiếp có nhiều vật thể bị che khuất (occlusion) và kích thước thay đổi nhanh, dễ phát sinh lỗi phân loại thuộc tính và sai lệch ranh giới box. | Ảnh chụp bounding box đối chiếu với nhãn reference, log lỗi ATTRIBUTE trong `findings.csv`, ghi chú về điều kiện che khuất. |

Giới hạn của kết luận từ ba frame ADASIND: Cỡ mẫu 3 frame chỉ mang tính chất chẩn đoán nhanh (spot-check) tại một số thời điểm cục bộ, chưa bao quát được toàn bộ phân phối dữ liệu (thay đổi về điều kiện ánh sáng, mật độ giao thông giờ cao điểm, hoặc các góc khuất camera khác nhau). Do đó, tỷ lệ lỗi ở đây đóng vai trò chỉ thị điểm nghẽn (defect pattern) chứ chưa đại diện cho phân phối lỗi tổng thể của toàn bộ tập dữ liệu lớn.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- **Cách soát độ phủ & tránh trùng lặp:** Áp dụng phương pháp lấy mẫu phân tầng (stratified sampling) kết hợp bước nhảy thời gian (temporal subsampling/stride tối thiểu 30-50 frame) giữa các frame liên tiếp trong cùng một chuỗi cảnh để triệt tiêu tương quan chuỗi thời gian (temporal correlation). Đồng thời phân bổ đều 200 frame trên cả 4 góc camera và các điều kiện môi trường khác nhau.
- **Vì sao chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:** Kế hoạch 200 frame này được thiết kế theo hướng tìm kiếm ca lỗi biên / ca khó (targeted/stratified inspection) nhằm phát hiện sớm các pattern vi phạm quy tắc, chứ không phải lấy mẫu ngẫu nhiên đồng nhất (uniform random sampling) trên toàn bộ phân phối dữ liệu vận hành. Do đó, nó không đáp ứng tính đại diện thống kê để suy rộng ra tỷ lệ lỗi thực tế (true error rate) của toàn hệ thống.
