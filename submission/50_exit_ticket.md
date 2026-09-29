# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời. Các chi tiết về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao? 
Cần một quy tắc riêng (Cross-camera / Seam Policy) thay vì vội vàng gán nhãn là lỗi `DUPLICATE`. Trong hệ thống 4 camera mắt cá (SVM), vùng seam là khu vực chồng lấn quang học (overlapping FOV). Khi vật thể nằm vắt ngang qua ranh giới cắt/ghép, cả hai camera đều thu nhận được các phần hình ảnh của cùng một thực thể ở hai góc nhìn và mức độ méo khác nhau. Nếu xem là DUPLICATE và tùy tiện xóa một box ở mức 2D cục bộ, downstream tracking/fusion có thể mất dấu hoặc ước lượng sai tâm thể tích 3D. Quy tắc riêng cần định rõ: hoặc gán nhãn trên không gian hợp nhất BEV, hoặc chỉ định camera chủ đạo (nơi vật thể hiển thị >50% diện tích) mang nhãn chính và liên kết nhãn phụ bằng cờ `cross_camera_ref`.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
- Giữ cùng track ID: Khi vật thể duy trì quỹ đạo chuyển động liên tục, nhận diện nhất quán và không bị mất dấu hoàn toàn quá thời gian quy định (thường <= 1-2 giây / 30 frames).
- Thêm keyframe: Khi vật thể thay đổi trạng thái hình học đột ngột (đổi hướng rẽ, tăng/giảm tốc, xoay thân xe làm thay đổi góc chiếu hộp bao, hoặc bắt đầu bị che khuất).
- Trạng thái Outside: Khi vật thể di chuyển ra khỏi trường nhìn (FOV) của camera hoặc bị che khuất hoàn toàn (100% occlusion) nhưng dự kiến sẽ xuất hiện lại.
- Bằng chứng cần trước khi nối track qua hai camera: (1) Tính liên tục về thời gian và không gian (spatio-temporal consistency) dựa trên ma trận ngoại suy calibration; (2) Sự tương đồng đặc trưng ngoại hình (appearance re-ID feature vector) và kích thước vật lý; (3) Hướng và vận tốc di chuyển phù hợp giữa điểm thoát (exit point) của camera này và điểm vào (entry point) của camera kế cận trên mặt phẳng BEV.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người khác coi là lỗi — bạn đã bảo vệ hay đổi ý, và vì sao?
Tại frame `adasind_019560.jpg` ở zone center block B2, ban đầu nhóm coi các bounding box ở hậu cảnh xa là đúng vì mắt thường vẫn nhận diện được vệt chuyển động của phương tiện. Tuy nhiên, sau khi sweep IoU và đối chiếu với model predictions, nhóm nhận thấy các đối tượng này bị che khuất trên 85% và kích thước dưới 15 pixel, không đủ đặc trưng ổn định để huấn luyện mô hình. Nhóm đã chủ động đổi ý, chấp nhận các box đó là SPURIOUS theo quy chuẩn khắt khe của reference, đồng thời ban hành bản vá guideline `20_guideline_patch.md` để chuẩn hóa ngưỡng nhận diện (occlusion tối đa 80% và kích thước tối thiểu). Việc đổi ý này giúp dữ liệu đạt tính nhất quán cao và giảm nhiễu cho mô hình thị giác máy tính.
