# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 432; đối tượng sau duyệt: 447; ngưỡng accepted: IoU >= 0.9
- Đã **loại 25 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 425 | 425/432 = 98.4% | 432 box máy |
| edited (sửa) | 1 | 1/432 = 0.2% | 432 box máy |
| deleted (xoá) | 6 | 6/432 = 1.4% | 432 box máy |
| added (vẽ thêm) | 21 | 21/447 = 4.7% | 447 đối tượng sau duyệt |

Trong số deleted + added, **1 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 20.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 246 | 1 | 1 | 1 |
| bicycle | 9 | 0 | 0 | 0 |
| motorbike | 88 | 0 | 1 | 6 |
| car | 50 | 0 | 0 | 12 |
| bus | 3 | 0 | 0 | 0 |
| truck | 5 | 0 | 2 | 1 |
| three_wheeler | 24 | 0 | 2 | 1 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 46 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 36 | 19 | 10 | 65.5% (KTC 95%: 52–77%) | 78.3% (KTC 95%: 64–88%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 11/12 = 91.7% (KTC 95%: 65–99%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - extra: 3/3
  - missing: 2/3
  - loose: 3/3
  - wrong_class: 3/3
  - lọt: img_024.jpg (missing)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
