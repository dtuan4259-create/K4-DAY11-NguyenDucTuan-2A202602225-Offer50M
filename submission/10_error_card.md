# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | MISSING | 11 |
| center | B2 | SPURIOUS | 12 |
| center | C0 | SPURIOUS | 2 |
| edge | B2 | SPURIOUS | 1 |
| mid | B2 | ATTRIBUTE | 1 |
| mid | B2 | MISSING | 4 |
| mid | B2 | SPURIOUS | 6 |
| unknown | B2 | ATTRIBUTE | 2 |
| unknown | B2 | WRONG_CLASS | 1 |
| unknown | C0 | MISSING | 3 |

## Top defects
- SPURIOUS: 21 (ví dụ frame adasind_019560.jpg)
- MISSING: 18 (ví dụ frame adasind_019560.jpg)
- ATTRIBUTE: 3 (ví dụ frame adasind_062370.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: Lỗi SPURIOUS (21 lỗi) và MISSING (18 lỗi) tập trung cao nhất ở zone center block B2 (frame adasind_019560.jpg). Nguyên nhân do ranh giới vật thể ở xa/bị che khuất chưa thống nhất trong guideline ban đầu, dẫn đến việc người gán nhãn bắt nhầm bóng cây/vật thể nền thành đối tượng (spurious) hoặc bỏ sót các phương tiện bị khuất tầm nhìn (missing).
- Cách sửa và ai nhận việc (`owner`): Rà soát lại toàn bộ box tại zone center trong slice B2, áp dụng ngưỡng IoU >= 0.5 và quy tắc occlusion tối thiểu. Giao Role B (QA/Rework) tiến hành drop các false positive box và bổ sung box thiếu theo reference.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): Frame adasind_019560.jpg trong findings.csv ghi nhận hàng loạt lỗi SPURIOUS và MISSING đối với class vehicle; quy tắc vi phạm theo guideline mục che khuất (occlusion > 80%) và bounding box tight-fit.
