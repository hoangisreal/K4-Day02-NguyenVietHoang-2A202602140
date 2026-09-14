# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Nguyễn Việt Hoàng<br>
**MSSV:** 2A202602140<br>
**Hình thức:** cá nhân — cá nhân hoặc theo cặp<br>
**Mã cặp:** SOLO — ghi `SOLO` nếu làm cá nhân

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
- Bốn mã ảnh: drive_008, drive_022, drive-033, drive_038
- Số vật thể thực tế: 87
- Mã SHA-256 của gói YOLO của bạn: cd085d828c7aecb0978bd287c5ea190f372ebdc7306a63c859ae84277dcc6941
- Mã SHA-256 của gói CVAT gốc của bạn: 54bf8a3fb51fe4675fb49706aff47c2ad3a8e20643413500ca10d3af24482373
- Nguồn đối chiếu: bạn cùng cặp hoặc bộ tham chiếu do người hướng dẫn thực hành cấp: SOLO
- Mã SHA-256 của gói đối chiếu: c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: ĐẠT kiểm tra nguồn đối chiếu | nguồn=teaching_reference | số vật thể=50

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu: hai mã sha-256 khác nhau

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_022/1 | car | sedan | clear, inside, confident |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

một chiếc xe được phân vào class car và nó có thuộc tính là clear, inside, confident

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| car | clear/truncated/needs_review| đuôi xe | van, clear/truncated/confident |

- Số hộp `needs_review` trước và sau khi kiểm:1/0
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: hình dáng xe còn mơ hồ chưa biết xe, sau khi review thì nhận ra hình dáng xe van không có cửa kính

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: lớp=2 (bus) | tâm=(0.3952, 0.7229) | kích thước=(0.4095, 0.3473)
- Tên lớp và tọa độ điểm ảnh `xyxy`: pixel xyxy: [121.9, 351.5, 384.0, 573.8]
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
đúng định dạng YOLO vẫn có thể sai nếu class ID không khớp, tọa độ vượt phạm vi, bounding box không ôm đúng vật thể


## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: drive_022, drive_033, drive_038 
- Mã ảnh thẩm định: drive_008
- Mô tả một dự đoán trong `detect_result.jpg`: mô hình YOLOv11 sau khi huấn luyện nhanh 8 epochs đã nhận diện được chiếc xe buýt lớn màu vàng-xanh
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào? - một vài xe có thể đi quá xa hoặc bị che/khuất thì mình cần phải xem xét thuộc tính occluded hay unclear
- Minh chứng nào có thể bác bỏ nhận định của bạn? - so sánh với teaching_reference
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế? 

dữ liệu còn quá ít dễ bị overfitting 

## 6. Đối chiếu nhãn

- Số hộp ghép được: 47
- IoU trung bình và trung vị: avg 0.860108, mean 0.875879
- Mức đồng thuận lớp: 0.744681
- Số hộp phía bạn không ghép được: 40
- Số hộp phía đối chiếu không ghép được: 3
- Một điểm khác biệt cụ thể: 50 box giống như ref, 40 box ko ref
- Quy tắc hoặc hành động sửa phát sinh: loại bỏ hoặc gán nhãn cho các phương tiện quá nhỏ, mờ ở hậu cảnh xa.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?

đồng thuận cao là nhiều người đồng ý chứ chưa hẳn là ground truth hoàn toàn

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

sự khớp nhau nhất quán gần như tuyệt đối giữa hai định dạng xuất khác nhau (YOLO và CVAT XML 1.1) với chỉ số IoU hình học tối thiểu đạt 0.999939
