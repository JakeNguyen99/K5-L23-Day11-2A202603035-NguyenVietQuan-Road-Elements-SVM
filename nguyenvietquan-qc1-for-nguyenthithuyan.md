# Phiếu QC 1 — Road v1.0

Lô được kiểm: Nguyen Thi Thuy An  
Người kiểm: Nguyen Viet Quan  
Ngày: 2026-11-22  
File và revision: `nguyenthithuyan-front.zip`, `nguyenthithuyan-fisheye.zip`, revision 1  
Số ảnh kiểm thủ công: 6 (3 front, 3 fisheye; không lấy ảnh extra)  
Số nhãn đã xem trong mẫu: 58 (34 shape + 24 tag)

Mẫu kiểm: A04, A05, A09, B02, B04, B08. Mẫu này được chọn để có cả hai nhóm camera và xem một số trường hợp lint cần tự kiểm. Đây là kiểm mẫu, không phải đối chiếu toàn bộ với GT.

## Tổng hợp theo mã

| Mã | Số mục kiểm | Số lỗi xác nhận | Mức |
| --- | ---: | ---: | --- |
| QC1-01 tag camera | 6 tag camera | 0 | nặng |
| QC1-02 đủ L1, R1 | 10 vị trí L1/R1 trên 5 ảnh camera trước/sau | 0; 3 vị trí chờ coach xác nhận | nặng |
| QC1-03 position | 10 lane | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | 10 lane | 0 | chính |
| QC1-05 vạch đứt, che | 10 lane | 0; cách biểu diễn vạch đôi cần coach xác nhận | chính |
| QC1-06 style, color, kind | 10 lane | 0 | chính |
| QC1-07 vùng lái được | 18 drivable/crosswalk/road_curb | 0 | chính |
| QC1-08 đèn, màu | 6 traffic_light | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | 6 traffic_light; không có traffic_sign trong mẫu | 0 | chính |
| QC1-10 hình in, đủ biển | 6 ảnh đã rà soát | 0 | chính |
| QC1-11 fisheye | 17 shape trong 3 ảnh fisheye | 0; 1 lane cần coach xem thêm | chính |
| QC1-12 file | 2 ZIP và 1 manifest | 0 | nhẹ / nặng |

Trong mẫu, chưa xác nhận được lỗi nặng hay lỗi chính nào. Tỉ lệ lỗi chính xác nhận: 0/58 = 0%. Tuy nhiên, chưa thể Release do còn các ca chờ coach xác nhận bên dưới.

## Ca chờ coach / lead

| Ảnh | Mã | Nội dung cần xác nhận |
| --- | --- | --- |
| A05 | QC1-02 | XML không có L1/R1. Ảnh mẫu không thấy rõ vạch sơn lane; coach cần xác nhận trường hợp miễn trước khi coi là không lỗi. |
| A09 | QC1-02 | XML không có R1. Cần coach xác nhận phía phải thật sự không có vạch lane nhìn thấy được hay bị sót. |
| A04 | QC1-05 | Có hai polyline cùng position L1 (yellow/double) và hai polyline cùng position R2 (white/single). Cần xác nhận quy ước hình học cho vạch đôi/song song; lint tự động không đủ để kết luận đây là tách sai. |
| A09 | QC1-05 | Hai polyline L2 đều gán `kind=double` và cùng position. Cần xác nhận có phải hai nét của một vạch đôi hay là hai annotation bị trùng. |
| B08 | QC1-11 | Lane `other` có 3 điểm. Hình chồng lớp cho thấy đường ngắn theo vùng cạnh; coach cần xác nhận số điểm này có đủ theo cung fisheye không. |

## Kiểm tra file và lint toàn lô

- Hai ZIP mở được, chỉ chứa `annotations.xml`, không kèm ảnh.
- Manifest revision 1; SHA-256 và dung lượng của cả hai ZIP khớp manifest.
- Cả 28 ảnh có đủ bốn tag; tag camera trong mẫu khớp tên/loại camera.
- `road_check lint` trên cả lô báo 5 nặng, 8 chính, 0 nhẹ. Cảnh báo nặng: A05 thiếu L1/R1, A07 thiếu L1, A09 thiếu R1, A14 thiếu R1. Cảnh báo chính: polyline cùng position ở A01/A04/A09/A11/A14; lane A15 có thể vẽ ngược chiều; lane B08 chỉ có 3 điểm. Đây là cảnh báo heuristic, không phải kết luận GT.
- A14 và A15/B08 nằm ngoài mẫu kiểm thủ công; không tính vào tỉ lệ 0/58 ở trên.

## Quyết định

**Hold** 