# Báo cáo Day 13 — Nguyễn Mạnh Dũng, 2A202603197

## 1. Kết luận trước

Tôi làm task `D13-2A202603197` (task 30) trên Windows, Chrome, CVAT 2.82.0. Lần xuất cuối hiện có **87 cuboid trên 10 frame, 6 cuboid có cảnh báo tự kiểm**. Tôi tự tin ở việc gán nhãn và xác định hướng; khó nhất là suy luận chiều dài xe khi một phần xe bị che khuất.

Số hộp theo frame: `s01f14`: 6; `s01f48`: 10; `s02f30`: 8; `s02f51`: 5; `s03f16`: 6; `s03f53`: 6; `s04f08`: 15; `s04f31`: 10; `s05f25`: 11; `s05f46`: 10. Frame ít hộp nhất là `s02f51` (5 hộp); tôi chưa ghi lại lý do cụ thể. Frame đầu `s01f14` có 6 hộp, **chưa đạt yêu cầu tối thiểu 8 hộp**. Tôi đã dừng bổ sung, không coi tổng số hộp cao là thay thế cho yêu cầu từng frame.

Thao tác CVAT: kéo chuột trái để xoay, chuột phải để dời, con lăn để phóng to/thu nhỏ. Vẽ cuboid dạng Shape, nháy đúp để đặt; chỉnh dài/rộng và yaw ở Top, cao/đáy ở Side và Front. Cuboid orientation hỗ trợ kiểm hướng; lệnh `preview` tạo ảnh đối chiếu cuboid trên camera.

## 2. Đọc frame (Bước 2)

| Frame | Hướng trên ảnh nhìn từ trên xuống | Mặt đường | Loại vật đã gán trong bảng A | Vật hai bên / điểm khó |
|---|---|---|---|---|
| s01f14 | Phía trước (−y) lên trên; +x sang trái; +y xuống dưới | Phẳng; chưa đo độ thấp so với LiDAR | Car, Bus | Hai bên có cây, không có rào chắn; tôi xác định được các cụm điểm đã quan sát |
| s01f48 | Chưa ghi nhận hướng theo một ảnh chụp cố định | Phẳng; chưa đo độ thấp so với LiDAR | Car, Cyclist | Hai bên có cây, không có rào chắn; nhiều ô tô phía trước che khuất nhau, khó xác định |

Tôi chưa đo tầm nhìn xa nhất nhận ra được trong hai frame. Khoảng cách lớn nhất của **các hộp đã gán**, không phải giới hạn tầm nhìn, là **34,61 m** ở `s01f14` và **22,68 m** ở `s01f48`. Mốc mặt đường khoảng −1,7 m là tham chiếu trong hướng dẫn, không phải phép đo riêng của tôi cho hai frame.

Camera giúp tôi nhận diện loại vật thể, màu sắc, chi tiết bề mặt và tín hiệu thị giác như biển báo, đèn, vạch kẻ đường. LiDAR hỗ trợ đo khoảng cách, tọa độ 3D và kích thước/hướng hình học theo mét. Tôi kết hợp camera để nhận loại vật với LiDAR để căn vị trí và kích thước cuboid; không khẳng định mỗi nguồn chỉ cung cấp được duy nhất các thông tin này. Đây là nhận xét chung, chưa có đủ ba ví dụ cụ thể cho từng nguồn trên hai frame.

| Tám frame còn lại | Hướng ảnh nhìn từ trên xuống / hiện tượng |
|---|---|
| s02f30, s02f51, s03f16, s03f53, s04f08, s04f31, s05f25 | Chưa có ghi chép riêng theo từng frame |
| s05f46 | Hai xe máy ở #8 và #9 đã được sửa từ Car thành Cyclist; khoảng cách 33,17 m và 33,56 m, thay cho ước lượng ban đầu khoảng 100 m |

Ảnh minh chứng mục này chưa được lưu đủ vào thư mục nộp. Bốn tên ảnh cần có: `t1-s01f14-3d.png`, `t1-s01f14-top.png`, `t1-s01f48-3d.png`, `t1-s01f48-top.png`.

## 3. Ba cuboid có hướng dẫn (Bước 3)

Ba hộp dưới đây được chọn từ bảng A để đối chiếu số đo; chưa xác nhận là đủ bộ ba hộp mẫu đã chụp theo quy trình của lớp.

| Vật đại diện | Frame, idx | Hướng / đáy | Kích thước dài × rộng × cao (m) và bảng 4.3 |
|---|---|---|---|
| Xe con gần | s01f48 #1, Car | Tôi đã hạ đáy và chỉnh sát đuôi; yaw 273,5°; chưa có xác nhận riêng về đủ ba view | 3,941 × 2,000 × 1,503. Rộng hơn khoảng sedan 1,70–1,90 m; chưa xác nhận kiểu xe và nguyên nhân |
| Xe buýt | s01f14 #3, Bus | Tôi đã hạ đáy; yaw 285,3°; chưa xác nhận hướng đầu xe và đủ ba view cho hộp mẫu | 10,765 × 2,779 × 3,143. Dài/cao trong khoảng xe buýt thường, rộng vượt 2,50–2,60 m |
| Xe hai bánh có người | s05f46 #8, Cyclist | Tôi xác nhận đã sửa nhãn; yaw 14,4°; chưa có xác nhận riêng về đủ ba view | 1,690 × 0,712 × 1,784. Theo khoảng xe máy có người, chiều dài dưới 1,8 m; rộng/cao trong khoảng |

Tại thời điểm viết báo cáo, coach chưa tổ chức vòng hiệu chuẩn trên task chung, nên tôi chưa có kết quả so sánh hoặc kết luận từ vòng này. Tôi chưa lập danh sách vật bỏ qua vì không đủ cơ sở. Các hộp vượt 50 m đã bị bỏ vì ngoài phạm vi, không phải vì đã xác nhận khó phân loại.

Đã lưu đủ ba ảnh mẫu: `t2-a-car.png` ở frame `s01f14`, `t2-b-bus-truck.png` ở frame `s03f16` và `t2-c-ped-cyclist.png` ở frame `s03f53`. Cần đối chiếu idx của các hộp trong ảnh với các hộp mẫu trong bảng trên.

## 4. Bốn hiện tượng chất lượng (Bước 4)

| Hiện tượng | Quan sát và nguồn | Giả thuyết / hệ quả khi gán |
|---|---|---|
| Bóng che | Ảnh `t4-1-bong-che.png`, task cá nhân, s04f08, do tôi chọn làm minh chứng bóng che; tôi cũng ghi nhận ô tô che khuất nhau ở s01f48 | Vật phía trước có thể chắn tia tới vật phía sau, làm khó suy luận chiều dài. Theo guideline, chỉ suy luận khi đủ cơ sở và bật Occluded cho hộp thực sự bị che |
| Thưa ở xa | Ảnh `t4-2-thua-o-xa.png`, task cá nhân, s04f31, do tôi chọn làm minh chứng; chưa đối chiếu idx và cự ly hai hộp trong ảnh | Khoảng cách tăng có thể làm giảm số điểm trên vật. Không dùng riêng khoảng cách để khẳng định vật nào bị thưa |
| Phản xạ thấp / ít điểm bất thường | Ảnh `t4-3-phan-xa-thap.png`, task cá nhân, s01f14, do tôi chọn làm minh chứng ít điểm; chưa xác định nguyên nhân | Không kết luận nguyên nhân phản xạ thấp chỉ từ số điểm ít; cần đối chiếu vật, cự ly, góc nhìn và che khuất |
| Điểm lạ | Ảnh `t4-4-diem-la.png`, task cá nhân, s02f51, do tôi chọn làm minh chứng; chưa khoanh rõ cụm điểm nghi vấn | Chưa có cơ sở kết luận nguyên nhân; điểm rời rạc không đủ bằng chứng thì không gán thành vật |

Đã lưu bốn ảnh hiện tượng: `t4-1-bong-che.png` ở s04f08, `t4-2-thua-o-xa.png` ở s04f31, `t4-3-phan-xa-thap.png` ở s01f14 và `t4-4-diem-la.png` ở s02f51. Chưa khoanh rõ vùng nghi vấn trên từng ảnh hoặc đối chiếu idx/cự ly các hộp minh họa; việc đủ ảnh không xác nhận các giả thuyết nguyên nhân. Điểm đang hiển thị màu trắng nên không dùng độ sáng trên ảnh để suy ra intensity. Cả bốn ảnh lấy từ task cá nhân.

## 5. Số từ cuboid của tôi (Bước 5)

Số liệu lấy từ `submission/D13-2A202603197-bangA.csv` và `submission/D13-2A202603197-tukiem.csv`, không sửa hai file xuất. Khoảng cách ngang tới tâm hộp là `dist_m = √(x²+y²)`; khoảng cách 3D tới tâm hộp là `r = √(x²+y²+z²)`.

| Cuboid | z (m) | h (m) | z đáy (m) | dist_m (m) | r (m) | yaw (°) | Đối chiếu đáy |
|---|---|---|---|---|---|---|---|
| s01f48 #1 Car | -0,970 | 1,503 | -1,721 | 6,65 | 6,717 | 273,5 | Trong ±0,3 m so với mốc −1,7 m; cần đối chiếu đường cục bộ |
| s01f14 #3 Bus | -0,225 | 3,143 | -1,797 | 28,46 | 28,463 | 285,3 | Trong ±0,3 m so với mốc −1,7 m; cần đối chiếu đường cục bộ |
| s05f46 #8 Cyclist | -0,646 | 1,784 | -1,538 | 33,17 | 33,174 | 14,4 | Trong ±0,3 m so với mốc −1,7 m; cần đối chiếu đường cục bộ |

Hộp gần nhất: **s03f16 #1 Truck**, dist_m **2,73 m**, r **2,833 m**. Xa nhất: **s05f46 #6 Truck**, dist_m **46,64 m**, r **46,637 m**.

Đối chiếu yaw: `s01f48 #7 Car` có yaw **270,1°**, gần hướng −y; `s02f51 #3 Car` có yaw **90,2°**, gần hướng +y. Đây là hướng của nhãn đã xuất, chưa thay thế việc kiểm đầu xe trên camera; hộp #7 có y dương nên không được gọi là xe ở phía trước chỉ dựa vào yaw.

Mật độ ở bốn hộp Car tại các cự ly khác nhau:

| Cuboid | dist_m (m) | points_in |
|---|---|---|
| s03f16 #2 Car | 7,93 | 2643 |
| s05f25 #6 Car | 17,24 | 1303 |
| s05f25 #7 Car | 28,05 | 297 |
| s04f31 #9 Car | 40,54 | 76 |

Trong bốn ví dụ này, từ 7,93 m tới 40,54 m, khoảng cách tăng khoảng 5,11 lần và số điểm giảm từ 2643 xuống 76, còn khoảng 2,88%. Xu hướng giảm phù hợp đường cong tham khảo trong bài giảng (1968, 718, 214, 72, 19 điểm ở các dải 0–10, 10–20, 20–30, 30–40, 60–80 m). Các hộp khác vật/frame nên không thể quy toàn bộ chênh lệch cho khoảng cách; ví dụ 40,54 m cũng không thuộc dải 30–40 m.

Sau tự kiểm, tôi đã hạ đáy Bus `s01f14 #3`; hạ đáy và chỉnh sát đuôi Car `s01f48 #1`; sửa nhãn `s05f46 #8, #9` thành Cyclist. Theo yêu cầu của tôi, công cụ trợ lý đã bỏ ba hộp ngoài phạm vi: `s05f46 #11, #12` của bản xuất trước (55,05 m và 57,71 m), và `s01f14 #7` của bản xuất trước (51,16 m). Tôi đã chạy lại `sync` sau thay đổi.

Sáu cảnh báo hiện còn:

| Hộp | Cảnh báo từ công cụ |
|---|---|
| s01f14 #6 Car | đáy cao hơn mặt đường 0.61 m (lơ lửng?) |
| s02f30 #8 Car | width=2.49 ngoài khoảng 1.4–2.3 của Car |
| s03f53 #4 Bus | đáy thấp hơn mặt đường 0.31 m (chìm?); width=3.52 ngoài khoảng 2.0–3.2 của Bus |
| s03f53 #5 Bus | đáy thấp hơn mặt đường 0.34 m (chìm?); width=3.52 ngoài khoảng 2.0–3.2 của Bus |
| s05f46 #2 Car | width=2.40 ngoài khoảng 1.4–2.3 của Car |
| s05f46 #5 Truck | đáy cao hơn mặt đường 0.68 m (lơ lửng?) |

Tôi đã xem lại các cảnh báo ở lần trước và giữ những hộp mà tôi thấy khớp vật. Riêng cảnh báo mới `s01f14 #6` chưa có xác nhận xem lại. Tôi không coi việc giữ hộp hay hết cảnh báo là chứng minh nhãn đúng.

Tôi đã kiểm chéo với học viên MSSV `2A202603219`, xem các hộp lần lượt từng frame; **chưa đối chiếu hai con số với bảng A**.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

Nhãn 3D dùng mét để mô tả vị trí và kích thước vật trong không gian thực; pixel thay đổi theo phối cảnh, khoảng cách và camera. Vật thưa điểm cần quy tắc riêng vì thiếu bằng chứng hình học, dễ gây sai tâm, chiều dài hoặc hướng; cần kết hợp camera và ghi mức độ chưa chắc thay vì kéo hộp theo phỏng đoán.

- **Công cụ AI:** ChatGPT/Codex.
- **Nội dung hỗ trợ:** thiết lập môi trường, giải thích thao tác CVAT, chỉnh hiển thị điểm, và tổng hợp CSV.
- **Cách dùng kết quả:** tôi vẽ cuboid và sửa nhãn trên CVAT, chạy lệnh check. AI tổng hợp CSV và đưa ra gợi ý hoàn thiện báo cáo.

Tôi còn chưa chắc chiều dài phần xe bị che khuất, kích thước một số hộp ngoài khoảng tham chiếu và sáu cảnh báo còn lại. Bài chưa đủ hai hộp ở frame đầu, mô tả vị trí cụ thể của bốn hiện tượng còn hạn chế, chưa có xác nhận đối chiếu hai số khi kiểm chéo. Đã lưu đủ 11 ảnh, gồm bốn ảnh đọc frame, ba ảnh cuboid mẫu và bốn ảnh hiện tượng; hai ảnh nhìn từ trên xuống chưa có chữ đánh dấu phía trước, +x, +y. Bộ ảnh được đóng gói thành `work/K5-L23-DAY13-2A202603197-images.zip` để nộp LMS.
