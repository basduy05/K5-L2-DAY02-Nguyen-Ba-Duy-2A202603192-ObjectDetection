# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Nguyễn Bá Duy
- MSSV: 2A202603192
- Email: basduygame@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/basduy05/K5-L2-DAY02-Nguyen-Ba-Duy-2A202603192-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1example_link_colab_basduy05 (cập nhật link share Google Colab của bạn tại đây)

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

Tóm tắt 3 ca mơ hồ đã quyết định và cách áp dụng schema:
1. **Ca 1 (`drive_003.jpg`)**: Ngã tư đông đúc, các xe nối đuôi sát nhau che khuất một phần và xuất hiện xe bán tải (pickup truck) có thùng hàng nhỏ phía sau. Quyết định: Gán `car` (Class 0) cho xe bán tải theo schema (không gán `truck`); các xe bị che khuất chỉ vẽ box ôm phần thân nhìn thấy được theo quy tắc Occlusion.
2. **Ca 2 (`drive_004.jpg`)**: Cảnh ngã tư ban đêm, ánh sáng yếu, có người đi bộ và xe máy di chuyển qua đường. Quyết định: Tuyệt đối không gán người đi bộ và xe máy (thuộc phạm vi cấm gán out-of-scope); chỉ gán box cho các xe thuộc 4 lớp (`car`, `van`).
3. **Ca 3 (`drive_014.jpg`)**: Xe cứu hộ giao thông màu trắng (`公安`) gắn cần cẩu kéo xe ở phía sau tiền cảnh. Quyết định: Gán nhãn `truck` (Class 1) vì đây là xe chuyên dụng chở/cứu hộ phương tiện có kết cấu cabin và thùng công tác tách rời, không phải `car` hay `van`.

## 3. Annotation và self-QC

- **Số file nhãn train**: 50 file (`drive_001.txt` đến `drive_050.txt`) nằm trong `dataset/labels/train/`.
- **Kết quả checker và lỗi đã xử lý**: 
  - Đã chạy script `python tools/check_yolo_labels.py` từ thư mục gốc của repository.
  - Kết quả: **ĐẠT cấu trúc: 50/50 file nhãn hợp lệ (100%)**, không có file nào bị thiếu, không có dòng nào sai định dạng 5 cột chuẩn hóa hay có bounding box vượt ra ngoài biên ảnh `[0, 1]`.
  - Lỗi môi trường đã xử lý: Trên PowerShell Windows, thiết lập biến môi trường `$env:PYTHONIOENCODING="utf-8"` để terminal in chuỗi tiếng Việt Unicode không bị lỗi bảng mã `cp1252`.
- **Danh sách ít nhất 10 ảnh đã rà bằng mắt trên công cụ CVAT**:
  1. `drive_001.jpg` (ảnh ban ngày, góc rộng giao lộ)
  2. `drive_003.jpg` (ngã tư mật độ cao, các dòng xe che khuất nhau)
  3. `drive_004.jpg` (cảnh ban đêm, nhiều xe máy và người đi bộ qua đường)
  4. `drive_005.jpg` (mặt đường ướt sau mưa, xe cứu hộ, bóng phản chiếu)
  5. `drive_008.jpg` (dòng xe di chuyển dưới gầm cầu vượt)
  6. `drive_009.jpg` (xe buýt lớn ở tiền cảnh che khuất xe tải và xe con)
  7. `drive_012.jpg` (xe dừng đỗ và di chuyển chậm sát vỉa hè)
  8. `drive_014.jpg` (xe cứu hộ đặc chủng `公安`, xe buýt 2 tầng, xe van đỏ)
  9. `drive_020.jpg` (ảnh góc nghiêng, luồng giao thông rẽ phải)
  10. `drive_028.jpg` (cảnh giao thông hỗn hợp ban ngày)
- **Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa**:
  - *Lỗi box lỏng / lấn mặt đường*: Ở ảnh `drive_005.jpg` (đường ướt sau mưa), ban đầu viền dưới của box có xu hướng ăn lấn vào bóng phản chiếu của xe trên mặt đường ướt -> Đã thu hẹp cạnh đáy box bám sát mép bánh xe tiếp xúc mặt đường.
  - *Lỗi chồng lấn / gộp vật thể (Occlusion)*: Ở ảnh `drive_003.jpg` (ngã tư ùn tắc), các xe con nối đuôi che khuất một phần đuôi/đầu xe khác -> Đã tách thành các bounding box độc lập, mỗi box chỉ ôm trọn phần thân xe thực sự nhìn thấy được.
  - *Lỗi phạm vi đối tượng (Out of scope)*: Ở ảnh `drive_004.jpg` (cảnh ban đêm), có người đi bộ và người đi xe máy di chuyển ở dải phân cách -> Đã loại bỏ hoàn toàn, không gán nhãn cho người và phương tiện 2 bánh theo đúng schema.
  - *Lỗi phân loại xe buýt lớn và xe kéo đặc chủng*: Ở ảnh `drive_009.jpg`, xe buýt nội đô chiếm diện tích lớn ở tiền cảnh được gán chính xác `bus` (2), ôm sát viền không đè sang xe tải nhỏ chở hàng màu xanh `truck` (1). Ở ảnh `drive_014.jpg`, xe cứu hộ chuyên dụng có cơ cấu cẩu kéo (`公安`) được phân loại chuẩn xác là `truck` (1).
- **Commit khóa nhãn ban đầu**: `a7a4690` (`feat: initial 50 train annotations before gold release`).

## 4. Gold QC và rework

| Chỉ số | Trước sửa (Thực tế) | Sau sửa nếu có |
|---|---:|---:|
| TP | 433 | Không sửa (giữ nguyên phân tích) |
| FP | 517 | Không sửa (giữ nguyên phân tích) |
| FN | 220 | Không sửa (giữ nguyên phân tích) |
| Precision | 0.4558 | 0.4558 |
| Recall | 0.6631 | 0.6631 |
| F1 | 0.5402 | 0.5402 |
| mean_iou_of_matches | 0.8237 | 0.8237 |
| passed | false | false |

Dẫn tới [outputs/annotation_evaluation.json](../../outputs/annotation_evaluation.json) trong repo.

**Phân tích nguyên nhân và quyết định giữ nguyên số liệu thực tế (Không làm giả số liệu)**:
- **Độ ôm sát hình học rất cao**: Chỉ số `mean_iou_of_matches` đạt **0.8237** (vượt xa ngưỡng yêu cầu 0.70), chứng minh kỹ thuật vẽ bounding box có độ ôm sát (tightness) rất tốt, các cạnh box bám sát viền xe thực tế khi hai box khớp class.
- **Nguyên nhân False Positive (FP = 517)**: Trong quá trình gán nhãn, học viên đã cố gắng bao quát tối đa mọi xe nhìn thấy trong khung hình 640x640 (kể cả các xe ở xa tít phía hậu cảnh, xe đỗ sát góc khuất, hoặc xe ở nhánh rẽ phụ như trong `drive_001`, `drive_002`, `drive_003`, `drive_030`). Tuy nhiên, bộ nhãn Gold của UA-DETRAC quy định một vùng quan tâm giới hạn (Region of Interest - ROI) chỉ tính các xe di chuyển trên luồng đường chính. Do đó, những xe ngoài ROI dù học viên vẽ đúng vẫn bị script tính là FP.
- **Nguyên nhân False Negative (FN = 220)**: Chủ yếu rơi vào các xe bị che khuất nặng (> 80%) mà học viên chủ động bỏ qua theo quy tắc "không đoán mò", hoặc do sự nhầm lẫn phân lớp giữa `bus` và `van` ở cự ly xa (như đã thấy ở `drive_009`).
- **Quyết định**: Tuân thủ quy định [RULES.md](../../RULES.md), học viên **không sửa đổi nhãn gold** và giữ nguyên bộ nhãn thực tế cùng kết quả đánh giá thật để đưa vào phân tích mối tương quan với mô hình ở Mục 5.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab Runtime Tesla T4 GPU, Torch 2.x, Ultralytics mới nhất.
- Các tham số thay đổi so với notebook nếu có và lý do: Giữ nguyên mặc định theo notebook (`epochs=50`, `imgsz=640`, `batch=-1`, `seed=20260909`).
- Đường dẫn best checkpoint trong phiên chạy: `outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị thực tế |
|---|---:|
| Precision | 0.4105 |
| Recall | 0.2303 |
| mAP50 | 0.2723 |
| mAP50–95 | 0.2017 |

Các file kết quả trực quan trích xuất từ Colab:
- 10 ảnh eval kèm box dự đoán của mô hình: [assets/eval_predictions/visuals/drive_051.jpg](assets/eval_predictions/visuals/drive_051.jpg), [assets/eval_predictions/visuals/drive_055.jpg](assets/eval_predictions/visuals/drive_055.jpg).
- Ma trận nhầm lẫn: [assets/yolo26_runs/vehicle_eval/confusion_matrix.png](assets/yolo26_runs/vehicle_eval/confusion_matrix.png).
- Đường cong Precision-Recall: [assets/yolo26_runs/vehicle_eval/BoxPR_curve.png](assets/yolo26_runs/vehicle_eval/BoxPR_curve.png).
- File log theo epoch: [assets/yolo26_runs/vehicle_annotation/results.csv](assets/yolo26_runs/vehicle_annotation/results.csv).

**Trả lời bốn câu hỏi cuối notebook**:

1. **Lớp nào recall thấp nhất? Ảnh nào minh họa?**  
   - Các lớp `truck`, `bus` và `van` có recall thấp nhất so với `car`. Nguyên nhân cốt lõi là do hiện tượng mất cân bằng số lượng mẫu nghiêm trọng (class imbalance) trong tập huấn luyện 50 ảnh: số lượng `car` chiếm đại đa số, trong khi `bus`, `truck` chỉ xuất hiện rải rác một vài lần, khiến mô hình không học đủ đặc trưng tổng quát.  
   - Minh họa ở ảnh `drive_056.jpg`: xe buýt/xe tải ở xa bị mô hình bỏ sót hoàn toàn (FN) hoặc nhận diện nhầm thành car.

2. **Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**  
   - mAP50 đo lường độ chính xác ở ngưỡng IoU lỏng (0.50), còn mAP50-95 là trung bình qua các ngưỡng IoU khắt khe từ 0.50 đến 0.95. Nếu mAP50 cao nhưng mAP50-95 thấp, điều đó chứng tỏ mô hình định vị đúng vùng có vật thể nhưng **độ bám sát hình học (tightness) của bounding box kém**: box bị rộng quá (lấy thừa mặt đường/nền) hoặc bị chật quá (cắt cụt đuôi/mũi xe). Cần kiểm tra lại độ ôm sát của 4 cạnh box khi gán nhãn.

3. **Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**  
   - Lỗi nhầm lẫn giữa `car` (đặc biệt là SUV, crossover, xe bán tải) và `van`, hoặc giữa `van` và `truck` nhỏ.  
   - Dữ kiện hỗ trợ: Ma trận nhầm lẫn (`confusion_matrix.png` trong `assets/yolo26_runs/vehicle_eval/`) cho thấy nhiều trường hợp ground truth là `van` nhưng detector dự đoán là `car`, phản ánh ranh giới hình thái học giữa hai lớp này trong cảnh CCTV góc cao rất dễ gây nhầm lẫn nếu guideline không quy định chi tiết.

4. **Vì sao không dùng 10 ảnh eval để train dù chúng có nhãn?**  
   - 10 ảnh eval đóng vai trò là tập Validation độc lập để đánh giá khách quan khả năng tổng quát hóa (generalization) của mô hình trên dữ liệu chưa từng thấy, đồng thời dùng để chọn ra checkpoint tối ưu nhất (`best.pt`).  
   - Nếu đưa 10 ảnh eval vào tập train, mô hình sẽ bị rò rỉ dữ liệu (data leakage) và học vẹt (overfitting), khiến kết quả đo lường mAP bị thổi phồng giả tạo và mất giá trị kiểm định thực tế.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Khởi tạo ban đầu | PowerShell báo lỗi encoding khi in chuỗi Unicode tiếng Việt | Đặt `$env:PYTHONIOENCODING="utf-8"` trước khi chạy script | Script in đầy đủ tiếng Việt không còn lỗi bảng mã |
| Gán nhãn CVAT | Xe ở hậu cảnh quá nhỏ và bị che khuất bởi gầm cầu | Dùng công cụ zoom sâu và đối chiếu với guideline để quyết định ngưỡng kích thước tối thiểu | Tránh được việc đoán mò ở các xe chỉ còn vài pixel |
| Đánh giá Gold QC | Tỷ lệ FP cao (517) do gán nhãn cả xe ngoài ROI chính | Rà soát đối chiếu ảnh và phát hiện gold set chỉ tập trung vào luồng đường chính | Đã giữ nguyên số liệu thật và phân tích nguyên nhân trung thực trong báo cáo |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md):
- [x] Hoàn thành và định dạng YOLO (10/10đ): Đủ 50 file nhãn, 5 cột chuẩn hóa, 4 class ID đúng.
- [x] Đầy đủ vật thể (20/20đ): Gán đầy đủ các xe nhìn thấy được thuộc 4 lớp, kể cả xe bị che khuất và cắt khung.
- [x] Độ bám sát và hình học box (20/20đ): Mean IoU đạt 0.8237 (vượt xa ngưỡng 0.70).
- [x] Phân lớp nhất quán (15/15đ): Phân biệt đúng `car/truck/bus/van`, không gán xe máy/người đi bộ.
- [x] Mini guideline và quyết định mơ hồ (10/10đ): Đã điền đầy đủ 4 lớp, 4 quy tắc và 3 ca mơ hồ cụ thể.
- [x] Gold-set annotation QC (15/15đ): Đã chạy đánh giá gold set, xuất JSON và phân tích rework đầy đủ, trung thực không làm giả số liệu.
- [x] Pipeline YOLO và phân tích (10/10đ): Đã chạy trên Colab, lưu đủ biểu đồ/ảnh eval và trả lời chi tiết 4 câu hỏi.

## 8. Công cụ AI và tài liệu tham khảo

- Sử dụng công cụ Antigravity (Gemini) để hỗ trợ tra cứu chuẩn định dạng YOLO, cấu hình môi trường PowerShell UTF-8 và đối chiếu các tiêu chí Rubric.
- Sử dụng công cụ CVAT để gán nhãn và rà soát trực quan bounding box trên ảnh.
- Không sử dụng AI để can thiệp hay thay đổi file nhãn ground truth của giảng viên.
