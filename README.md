================================================================
  HỆ THỐNG CHẨN ĐOÁN CẢM CÚM & DỊ ỨNG — TỔNG HỢP THÔNG TIN
================================================================

TỔNG QUAN
---------
Tên ứng dụng : Hệ Thống Chẩn Đoán Cảm Cúm & Dị Ứng
Loại         : Web app HTML đơn file (HTML + CSS + JavaScript)
Ngôn ngữ     : Tiếng Việt
Giao diện    : Dark theme, font Be Vietnam Pro + Playfair Display
Màu nền      : #0d1117 (tối), gradient động phía sau


================================================================
GIAO DIỆN & UX
================================================================
- Thanh tiến trình 3 bước (progress bar) hiển thị mức hoàn thành
- Các card bo tròn với hover effect và animation slide-up/fade
- Nút chẩn đoán bị vô hiệu hóa cho đến khi chọn ít nhất 1 triệu chứng
- Kết quả hiện ra bên dưới và tự cuộn đến phần kết quả
- Cảnh báo khẩn cấp hiện hộp đỏ khi có triệu chứng nguy hiểm


================================================================
LUỒNG NHẬP LIỆU — 3 BƯỚC
================================================================

BƯỚC 1 — TRIỆU CHỨNG (checkbox, chọn nhiều)
--------------------------------------------
Nhóm Cảm Cúm:
  [sot]       Sốt cao >= 38°C
  [dau_co]    Đau cơ / mỏi người
  [dau_dau]   Đau đầu dữ dội
  [met_moi]   Mệt mỏi / kiệt sức
  [ho_khan]   Ho khan / ho nhiều
  [dau_hong]  Đau họng

Nhóm Dị Ứng:
  [hat_hoi]   Hắt hơi liên tục
  [ngua_mat]  Ngứa mắt / chảy nước mắt
  [ngua_mui]  Ngứa / chảy nước mũi
  [phat_ban]  Phát ban / nổi mề đay
  [ngua_da]   Ngứa da / nổi mẩn
  [kho_tho]   Khó thở / tức ngực
  [nghet_mui] Nghẹt mũi
  [buon_non]  Buồn nôn / ói mửa

BƯỚC 2 — MỨC ĐỘ TRIỆU CHỨNG (radio, chọn 1)
---------------------------------------------
  [nhe]   Nhẹ   — không ảnh hưởng sinh hoạt
  [vua]   Vừa   — hơi khó chịu
  [nang]  Nặng  — ảnh hưởng nhiều

BƯỚC 3 — THỜI GIAN XUẤT HIỆN (radio, chọn 1)
---------------------------------------------
  [1ngay]       Dưới 1 ngày
  [3ngay]       1–3 ngày
  [7ngay]       4–7 ngày
  [14ngay]      Hơn 1 tuần
  [thuongxuyen] Thường xuyên / theo mùa


================================================================
KNOWLEDGE BASE — 11 LUẬT IF-THEN
================================================================

--- LUẬT CẢM CÚM ---

R1 | Cảm cúm điển hình
   Điều kiện : sốt AND đau cơ AND mệt mỏi
   Kết luận  : flu
   Trọng số  : 3

R2 | Cúm có ho và đau họng
   Điều kiện : sốt AND ho khan AND đau họng
   Kết luận  : flu
   Trọng số  : 2.5

R3 | Cúm với đau đầu
   Điều kiện : sốt AND đau đầu dữ dội AND mệt mỏi
   Kết luận  : flu
   Trọng số  : 2

R4 | Cúm đường tiêu hóa
   Điều kiện : sốt AND buồn nôn AND mệt mỏi
   Kết luận  : flu
   Trọng số  : 1.5

--- LUẬT DỊ ỨNG ---

R5 | Dị ứng đường hô hấp
   Điều kiện : hắt hơi AND ngứa mũi AND ngứa mắt
   Kết luận  : allergy
   Trọng số  : 3

R6 | Dị ứng da
   Điều kiện : phát ban AND ngứa da
   Kết luận  : allergy
   Trọng số  : 2.5

R7 | Dị ứng mũi đơn thuần
   Điều kiện : hắt hơi AND nghẹt mũi AND ngứa mũi AND NOT sốt
   Kết luận  : allergy
   Trọng số  : 2

R8 | Dị ứng hen suyễn
   Điều kiện : khó thở AND (ngứa da OR phát ban)
   Kết luận  : allergy
   Trọng số  : 2

R9 | Dị ứng theo mùa
   Điều kiện : hắt hơi AND ngứa mắt AND thời gian = thường xuyên
   Kết luận  : allergy
   Trọng số  : 2.5

--- LUẬT CẢ HAI ---

R10 | Cúm kèm dị ứng
    Điều kiện : sốt AND hắt hơi AND ngứa mắt
    Kết luận  : both
    Trọng số  : 2

R11 | Tổng hợp nhiều triệu chứng
    Điều kiện : >= 2 triệu chứng cúm AND >= 2 triệu chứng dị ứng
    Kết luận  : both
    Trọng số  : 2


================================================================
ENGINE CHẨN ĐOÁN — CÁCH TÍNH ĐIỂM
================================================================

Mỗi luật thỏa điều kiện → cộng trọng số vào nhóm tương ứng (flu / allergy / both)

Hệ số điều chỉnh:
  - Mức độ Nặng (nang)                          : trọng số × 1.3
  - Mức độ Nhẹ  (nhe)                           : trọng số × 0.8
  - Thời gian thường xuyên + luật dị ứng        : trọng số × 1.3
  - Thời gian 7 ngày hoặc 14 ngày + luật cúm    : trọng số × 1.1

Xác định kết quả:
  1. Nếu scores.both > max(flu, allergy) × 0.7  → kết luận: BOTH (cả hai)
  2. Nếu scores.flu >= scores.allergy            → kết luận: FLU (cảm cúm)
  3. Ngược lại                                   → kết luận: ALLERGY (dị ứng)
  4. Không có luật nào kích hoạt                 → kết luận: HEALTHY (không rõ)

Công thức độ tin cậy:
  confidence = min(round((maxScore / (total + 2)) × 100 + 30), 92)
  Tối đa 92% — không áp dụng cho trường hợp "healthy"


================================================================
KẾT QUẢ CHẨN ĐOÁN — 4 TRƯỜNG HỢP
================================================================

1. FLU — Cảm Cúm (màu #ff7b72)
   "Có dấu hiệu Cảm Cúm — Nhiễm virus influenza, phổ biến mùa lạnh và giao mùa"
   Khuyến nghị:
     - Nghỉ ngơi đầy đủ: ngủ đủ giấc, tránh hoạt động nặng
     - Uống nhiều nước: bù nước để hạ sốt, làm loãng dịch hô hấp
     - Theo dõi thân nhiệt: dùng Paracetamol khi sốt trên 38.5°C
     - Đeo khẩu trang: tránh lây lan cho người già, trẻ em
     - Thăm khám nếu sốt trên 39°C liên tục hơn 3 ngày

2. ALLERGY — Dị Ứng (màu #79c0ff)
   "Có dấu hiệu Dị Ứng — Phản ứng miễn dịch với phấn hoa, bụi, thức ăn, thuốc..."
   Khuyến nghị:
     - Xác định dị nguyên: phấn hoa, lông thú, thức ăn, thuốc...
     - Tránh tiếp xúc: vệ sinh môi trường, dùng máy lọc không khí
     - Thuốc kháng histamine: Cetirizine, Loratadine
     - Nhỏ mắt nếu cần: nước muối sinh lý hoặc thuốc nhỏ mắt kháng dị ứng
     - Test dị ứng: skin test hoặc IgE để xác định chính xác dị nguyên

3. BOTH — Cúm + Dị Ứng (màu #ffa657)
   "Có thể là Cúm và/hoặc Dị Ứng — Triệu chứng trùng lặp, cần đánh giá thêm"
   Khuyến nghị:
     - Theo dõi kỹ: ghi chép thời điểm và yếu tố kích hoạt
     - Khám bác sĩ để phân biệt: xét nghiệm máu hoặc test dị ứng
     - Uống đủ nước: hydrat hóa giúp cả hai tình trạng
     - Nhật ký triệu chứng: ghi thời điểm, hoàn cảnh, môi trường
     - Dùng thuốc đúng cách: không tự ý kết hợp nhiều loại thuốc

4. HEALTHY — Không rõ bệnh (màu #56d364)
   "Chưa đủ cơ sở chẩn đoán — Triệu chứng không khớp rõ ràng"
   Khuyến nghị:
     - Tiếp tục theo dõi: quan sát thêm trong 24–48 giờ
     - Nghỉ ngơi đủ giấc: 7–8 tiếng, giảm stress
     - Dinh dưỡng tốt: bổ sung vitamin C và kẽm
     - Vận động nhẹ nhàng: tăng cường miễn dịch


================================================================
CẢNH BÁO KHẨN CẤP
================================================================
Hiện hộp cảnh báo đỏ khi thỏa một trong các điều kiện sau:
  - Khó thở kết hợp với sốt HOẶC phát ban
  - Mức độ triệu chứng chọn là Nặng

Nội dung cảnh báo:
  "Triệu chứng của bạn có thể nghiêm trọng. Vui lòng đến cơ sở y tế
   sớm nhất có thể hoặc gọi đường dây y tế khẩn cấp nếu khó thở nặng."


================================================================
DISCLAIMER
================================================================
Đây là công cụ hỗ trợ tham khảo, KHÔNG thay thế tư vấn y tế chuyên nghiệp.
Vui lòng gặp bác sĩ nếu triệu chứng nghiêm trọng hoặc kéo dài.

================================================================
