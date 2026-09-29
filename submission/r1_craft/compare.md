# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_062370.jpg
- L2+R4 mid WRONG_CLASS
- R7 center MISSING
- R8 center MISSING
## adasind_086220.jpg
- L4+R1 mid ATTRIBUTE
## adasind_117120.jpg
- L4 center SPURIOUS
- R3 center MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 13 | 10 | 3 | 1 |
| mid | 6 | 5 | 1 | 1 |
| edge | 1 | 1 | 0 | 0 |
